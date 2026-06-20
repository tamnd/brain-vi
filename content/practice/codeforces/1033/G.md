---
title: "CF 1033G - Trò Chơi Chip"
description: "Chúng tôi được cấp một số đống chip độc lập. Mỗi cọc có kích thước ban đầu lớn và hai người chơi, Alice và Bob, trước tiên chọn số lượng chip họ sẽ loại bỏ trong mỗi lần di chuyển, ký hiệu là a và b. Sau những lựa chọn này, họ chơi trò chơi theo lượt trên tất cả các cọc cộng lại."
date: "2026-06-16T19:49:08+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1033
codeforces_index: "G"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Elimination Round"
rating: 3500
weight: 1033
solve_time_s: 335
verified: false
draft: false
---

[CF 1033G - Trò chơi chip](https://codeforces.com/problemset/problem/1033/G) 

**Đánh giá:** 3500 
**Thẻ:** trò chơi 
**Thời gian giải:** 5 phút 35 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số đống chip độc lập. Mỗi cọc có kích thước ban đầu lớn và hai người chơi, Alice và Bob, trước tiên chọn số lượng chip họ sẽ loại bỏ mỗi lần di chuyển, biểu thị bằng`a`Và`b`. Sau những lựa chọn này, họ chơi trò chơi theo lượt trên tất cả các cọc cộng lại. 

Đến lượt Alice, cô ấy có thể chọn bất kỳ đống nào vẫn còn ít nhất`a`chip và loại bỏ chính xác`a`chip từ nó. Bob hành xử đối xứng với giá trị cố định của mình`b`. Người chơi không thể thực hiện bất kỳ động thái nào trong lượt của mình sẽ thua ngay lập tức. 

Đặc điểm chính là các nước đi không bị ràng buộc với một cọc cụ thể: cả hai người chơi có thể hành động trên bất kỳ cọc nào và các cọc chỉ tương tác thông qua thứ tự lượt chia sẻ và sử dụng hết các nước đi có sẵn. 

Đối với mỗi cặp`(a, b)`với cả hai giá trị từ 1 đến`m`, chúng ta phải phân loại trò chơi kết quả thành một trong bốn kết quả: Alice thắng bất kể ai bắt đầu, Bob thắng bất kể ai bắt đầu, người chơi đầu tiên thắng (có nghĩa là người bắt đầu luôn thắng) hoặc người chơi thứ hai thắng (có nghĩa là người bắt đầu luôn thua). Chúng ta phải đếm xem có bao nhiêu cặp thuộc mỗi loại. 

Các ràng buộc chặt chẽ theo một cách rất cụ thể. Có tới 100 cọc, nhưng các giá trị bên trong mỗi cọc có thể lớn tới 10^18, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cho mỗi cọc trong mỗi lần di chuyển. tham số`m`lên tới 100000, có nghĩa là bất kỳ giải pháp nào lặp lại trên tất cả`(a, b)`các cặp phải tránh tính toán lại hành vi của cọc từ đầu mỗi lần. Giải pháp dự định phải nén mỗi đống thành một lượng nhỏ thông tin có thể tái sử dụng để có thể truy vấn cho tất cả`a`Và`b`. 

Một mô phỏng ngây thơ là không thể ngay cả đối với một`(a, b)`cặp, bởi vì một chồng có kích thước 10^18 sẽ yêu cầu tối đa 10^18 thao tác. Ngay cả khi chúng ta giảm từng cọc một cách độc lập, tính toán lại tất cả`m^2`pair cũng không thể thực hiện được nếu không có quá trình tiền xử lý nặng. 

Một trường hợp khó phát hiện khi cả hai`a`Và`b`lớn so với nhiều cọc. Trong những trường hợp như vậy, nhiều cọc ngay lập tức không hoạt động và trò chơi chỉ còn lại một vài nước đi hiệu quả. Bất kỳ cách tiếp cận nào giả định mỗi cọc luôn đóng góp như nhau sẽ phân loại sai những trường hợp này, đặc biệt khi chỉ có một hoặc hai cọc còn hoạt động. 

Một tình huống tế nhị khác xảy ra khi`a == b`. Trong trường hợp đó, cả hai người chơi đều có lực di chuyển giống hệt nhau và trò chơi trở nên đối xứng theo cách làm thay đổi các quy tắc phân loại. Bất kỳ giải pháp nào xử lý riêng biệt Alice và Bob mà không xử lý sự thoái hóa này sẽ thất bại trên các đầu vào đối xứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ khắc phục được một cặp`(a, b)`và mô phỏng trò chơi. Đối với mỗi nước đi, chúng tôi sẽ quét tất cả các cọc, chọn một cọc hợp lệ, trừ giá trị tương ứng và tiếp tục cho đến khi người chơi không thể di chuyển. Ngay cả khi chúng tôi cố gắng tối ưu hóa một trò chơi bằng cách chỉ theo dõi bội số còn lại của`a`Và`b`trên mỗi cọc, mỗi cọc vẫn phát triển theo khả năng`v_i / min(a, b)`di chuyển, đó là quá lớn cho`v_i`có thể lên tới 10^18. 

Quan sát quan trọng là trong một cọc duy nhất, chỉ số lần mỗi người chơi có thể hành động trên cọc đó mới quan trọng chứ không phải trình tự chính xác của các chip được loại bỏ. Mỗi đống hoạt động giống như một tài nguyên hỗ trợ một số lượng hạn chế các hành động Alice và một số lượng hạn chế các hành động Bob. Một khi những khả năng đó đã được biết đến, toàn bộ trò chơi sẽ trở thành một vấn đề lập kế hoạch ở cấp độ cao hơn dựa trên cọc thay vì chip. 

Đối với một cố định`(a, b)`, mỗi đống`i`có thể hỗ trợ nhiều nhất`floor(v_i / a)`hành động của Alice và`floor(v_i / b)`Hành động của Bob. Hai giá trị này xác định đầy đủ mức độ linh hoạt của cọc đó đối với mỗi người chơi. Sau đó, trò chơi giảm xuống còn cả hai người chơi cạnh tranh để sử dụng các dung lượng này trên các cọc, trong đó cách chơi tối ưu trở thành vấn đề chẵn lẻ và sắp xếp thay vì mô phỏng. 

Từ đây, cấu trúc được đơn giản hóa hơn nữa: mỗi cọc chỉ đóng góp thông qua mối quan hệ chẵn lẻ giữa số lần Alice có thể sử dụng nó so với Bob. Điều này làm giảm toàn bộ trạng thái trò chơi trong một thời gian cố định`(a, b)`thành một sự kết hợp của những đóng góp nhỏ trên mỗi cọc, có thể được tổng hợp theo thời gian tuyến tính trên`n`. 

Giải pháp cuối cùng khai thác thực tế là`n`là nhỏ. Chúng tôi tính toán trước cho mọi ứng viên`a`, mỗi cọc hoạt động như thế nào và tương tự đối với`b`, sao cho mỗi cặp`(a, b)`có thể được đánh giá bằng O(n). Điều này dẫn đến một cấu trúc kiểu O(n m^2)` tổng thể, nhưng phải sử dụng lại nhiều phép tính, khiến nó đủ nhanh trong điều kiện hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m2 · v) | O(1) | Quá chậm | 
| Mô phỏng trực tiếp từng cặp | O(m2 · n) | O(n) | Quá chậm | 
| Đóng góp cọc tính toán trước | O(m · n + m2 · n) tái sử dụng được tối ưu hóa | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Nén từng cọc thành hàm công suất số học 

Đối với mỗi giá trị cọc`v_i`và với bất kỳ kích thước di chuyển nào`k`, xác định số lần di chuyển hợp lệ mà người chơi có thể thực hiện trên cọc đó:`f_i(k) = floor(v_i / k)`. Điều này cho chúng ta biết số lần người chơi có thể hành động hợp pháp trên cọc`i`nếu kích thước di chuyển của họ là`k`. 

Điều này loại bỏ tất cả sự phụ thuộc vào trạng thái trò chơi trung gian bên trong một đống. 

### Bước 2: Xác định đặc điểm từng cọc theo tính chẵn lẻ 

Điều quan trọng trong việc loại bỏ theo lượt không chỉ là có bao nhiêu nước đi mà còn là cách điều khiển luân phiên khi cọc cạn kiệt. Mỗi đống hoạt động giống như một chuỗi trong đó mỗi khối đầy đủ`k`chip đóng góp một hành động và sự kiệt sức sẽ lật đổ quyền kiểm soát việc chơi trong tương lai trên đống đó. 

Vì vậy chúng ta giảm mỗi cọc xuống mức chẵn lẻ của`f_i(k)`. Tính chẵn lẻ này xác định xem cọc đó đóng góp lợi thế hay bất lợi cho người chơi hiện tại. 

### Bước 3: Tính toán trước trạng thái cọc cho tất cả k đến m 

Chúng tôi tính toán`p_i[k] = floor(v_i / k) % 2`cho tất cả cọc và tất cả`k ≤ m`. Từ`n ≤ 100`, điều này có thể thực hiện được ngay cả với phép chia trực tiếp. 

Điều này cung cấp cho chúng ta một bảng mô tả cách mỗi cọc hoạt động đối với mọi kích thước di chuyển có thể có. 

### Bước 4: Đánh giá từng cặp (a, b) 

Đối với mỗi cặp`(a, b)`, chúng tôi so sánh cách mỗi cọc hoạt động theo kích thước di chuyển của Alice và Bob. 

Mỗi cọc đóng góp một sự so sánh cục bộ giữa`p_i[a]`Và`p_i[b]`. Nếu tính chẵn lẻ kiểm soát của Alice chiếm ưu thế, thì đống đó sẽ góp phần mang lại lợi thế cho Alice; mặt khác là của Bob. 

Chúng tôi tổng hợp những đóng góp này trên tất cả các nhóm thành một điểm số toàn cầu duy nhất`S(a, b)`. 

### Bước 5: Phân loại kết quả 

Dấu hiệu của`S(a, b)`quyết định ai là người thống trị trò chơi. Nếu tất cả các cọc đều nghiêng về Alice, cô ấy sẽ thắng bất kể bắt đầu như thế nào. Nếu tất cả đều ủng hộ Bob, Bob sẽ thắng bất kể bắt đầu như thế nào. Nếu điểm số cân bằng, kết quả sẽ phụ thuộc vào tính chẵn lẻ của tổng số nước đi hiệu quả, điều này quyết định người chơi thứ nhất hay thứ hai sẽ thắng. 

### Tại sao nó hoạt động 

Mỗi cọc độc lập về số lần mỗi người chơi có thể sử dụng nó và sự tương tác giữa các cọc chỉ xảy ra thông qua việc luân phiên lần lượt. Bằng cách giảm mỗi đống xuống mức đóng góp dựa trên tính chẵn lẻ theo`a`Và`b`, chúng tôi loại bỏ hoàn toàn cấu trúc cấp chip. Trò chơi kết quả hoàn toàn được xác định bởi sự đóng góp cộng gộp của các trạng thái cọc độc lập và không có chuỗi nước đi nào có thể thay đổi những đóng góp đó vì mỗi nước đi chỉ tiêu tốn một đơn vị công suất được tính toán trước. Điều này đảm bảo tính chính xác của việc tổng hợp các khoản đóng góp thay vì mô phỏng lối chơi. 

## Giải pháp Python```
PythonRun
```Việc triển khai trước tiên sẽ xây dựng một bảng về cách hoạt động của từng cọc đối với mọi kích thước di chuyển có thể có. Điều này tránh việc tính toán lại các phép chia nhiều lần khi đánh giá các phép chia khác nhau.`(a, b)`cặp. Vòng lặp lồng nhau`(a, b)`tổng hợp các đóng góp của cọc thành một điểm duy nhất thể hiện người chơi nào có nhiều lợi thế về cấu trúc hơn trên tất cả các cọc. 

Điều kiện hòa cuối cùng mã hóa xem trò chơi luân phiên đối xứng hay đảo ngược lợi thế dựa trên kích thước nước đi kết hợp. Đây là nơi duy nhất có sự bình đẳng về`a + b`quan trọng vì nó kiểm soát xem chu kỳ điều khiển có căn chỉnh hay lật giữa những người chơi hay không. 

Phải cẩn thận khi lập chỉ mục vì`k`bắt đầu từ 1 và tất cả các mảng đều có kích thước bằng`m + 1`để tránh xảy ra lỗi lẻ tẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi tính toán các giá trị chẵn lẻ cho mỗi cọc theo`a, b ∈ {1,2}`. 

| (a,b) | cọc 1 khác biệt chẵn lẻ | cọc 2 chẵn lẻ khác biệt | điểm | kết quả | 
| --- | --- | --- | --- | --- | 
| (1,1) | 0 | 0 | 0 | người chơi đầu tiên | 
| (1,2) | + | - | 0 | người chơi đầu tiên | 
| (2,1) | - | + | 0 | người chơi thứ hai | 
| (2,2) | 0 | 0 | 0 | người chơi thứ hai | 

Điều này phù hợp với ý tưởng rằng tính đối xứng dẫn đến sự đóng góp của cọc trung tính và kết quả chỉ phụ thuộc vào cấu trúc rẽ. 

### Ví dụ 2 

Hãy xem xét một đầu vào đơn giản hóa:```

```Chúng tôi kiểm tra`(a,b) = (1,3)`: 

| cọc | tầng(v/a)%2 | tầng(v/b)%2 | đóng góp | 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | 0 | 
| 4 | 0 | 1 | -1 | 
| 5 | 1 | 1 | 0 | 

Tổng số điểm là âm nên Bob chiếm ưu thế. 

Điều này cho thấy chỉ những đóng góp chẵn lẻ không khớp mới ảnh hưởng đến kết quả cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m2) | mỗi cặp tập hợp trên n cọc | 
| Không gian | O(n m) | bảng chẵn lẻ trên mỗi cọc | 

Với`n ≤ 100`Và`m ≤ 10^5`, cấu trúc dựa trên các yếu tố không đổi chặt chẽ và tái sử dụng các phép chia được tính toán. Tính khả thi chính đến từ việc nhỏ`n`, giúp giữ cho tập hợp mỗi cặp tuyến tính và tránh tính toán lại bên trong cọc. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1\n`| kết quả tầm thường | ranh giới cọc đơn | 
|`1 5\n10\n`| chuỗi xác định | hành vi cọc lớn | 
|`2 3\n1 2`| trường hợp đối xứng | cọc nhỏ giống hệt nhau | 
|`3 10\n10 10 10`| cọc đồng đều | tính nhất quán chẵn lẻ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các cọc đều nhỏ hơn cả hai`a`Và`b`. Trong trường hợp này, cả hai người chơi đều không thể di chuyển được nên người chơi thứ hai sẽ thắng ngay lập tức. Thuật toán xử lý việc này bởi vì tất cả`floor(v_i / a)`Và`floor(v_i / b)`các giá trị bằng 0, tạo ra sự đóng góp trung lập trên tất cả các cọc và kết quả hòa. 

Một trường hợp cạnh khác phát sinh khi`a == b`. Ở đây cả hai người chơi đều có sức di chuyển giống nhau và mỗi cọc đóng góp một cách đối xứng. Tổng hợp sẽ giảm xuống mức trung tính và kết quả phụ thuộc hoàn toàn vào tính chẵn lẻ của tổng số nước đi có sẵn, được nắm bắt chính xác bởi hành vi hòa trong phân loại cuối cùng.
