---
title: "CF 102215J - Sức mạnh của mặt tối - 2"
description: "Mỗi Jedi có ba tham số nguyên không âm và tổng công suất của chúng được cố định sau khi Jedi được chọn. Sau khi biến Jedi thành mặt tối, chúng ta có thể phân phối lại tổng sức mạnh của Jedi đó một cách tùy ý trước mỗi trận chiến."
date: "2026-08-18T22:04:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "J"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 118
verified: false
draft: false
---

[CF 102215J - Sức mạnh của mặt tối - 2](https://codeforces.com/problemset/problem/102215/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi Jedi có ba tham số nguyên không âm và tổng công suất của chúng được cố định sau khi Jedi được chọn. Sau khi biến Jedi thành mặt tối, chúng ta có thể phân phối lại tổng sức mạnh của Jedi đó một cách tùy ý trước mỗi trận chiến. Phép phân phối lại có thể thay đổi cả ba tọa độ nhưng tổng của chúng phải bằng tổng ban đầu. 

Một cuộc chiến sẽ thắng khi Jedi được sửa đổi lớn hơn đối thủ ở ít nhất hai tọa độ tương ứng. Đối với mỗi Jedi ban đầu, chúng tôi cần số lượng Jedi khác mà một số phân phối lại hợp lệ cho phép họ giành chiến thắng. 

Hạn chế chính là quy mô của giải đấu. Với tối đa 500.000 Jedi, việc so sánh mỗi cặp sẽ cần khoảng 

2 500000⋅499999 ​ ≈1,25⋅10 11 

so sánh theo cặp. Giới hạn 2 giây loại trừ mọi thứ bậc hai. Chúng ta cần giảm mỗi đối thủ xuống một giá trị số nhỏ và sau đó trả lời tất cả các truy vấn bằng cách sắp xếp và tìm kiếm nhị phân. 

Bản thân các tham số có thể lớn bằng 10 9, do đó tổng có thể đạt tới 3⋅10 9. Số nguyên Python xử lý việc này một cách trực tiếp, trong khi việc triển khai C++ sẽ cần số nguyên 64 bit. 

Có một số sai lầm ranh giới dễ dàng. Vấn đề bất bình đẳng nghiêm ngặt. Với một Jedi (1,1,2), câu trả lời là 0 vì không còn ai để chiến đấu. Với hai Jedi giống hệt nhau (1,1,2), mỗi câu trả lời là một: một Jedi có tổng số 4 có thể phân phối lại thành (2,2,0), lớn hơn rất nhiều trong hai tọa độ đầu tiên. Một giải pháp sử dụng`<`thay vì`<=`vì ngưỡng sẽ từ chối trường hợp này một cách không chính xác. 

Số không cũng quan trọng. Coi như```

```Đầu ra đúng là`2 1 0`. Jedi đầu tiên có tổng cộng 3 và có thể đánh bại cả Jedi khác. Ví dụ, đối với (0,1,1), nó có thể sử dụng (1,2,0). Một giải pháp bất cẩn giả định tất cả các tham số đều dương có thể tính sai cặp tối thiểu. 

Cuối cùng, Jedi đã được sửa đổi không được phép coi là đối thủ. Nếu tìm kiếm nhị phân đếm mọi Jedi thỏa mãn điều kiện số thì chính Jedi ban đầu có thể được bao gồm. Số lượng đó phải được loại bỏ chính xác một lần. 

## Phương pháp tiếp cận 

Giải pháp vũ phu xem xét mọi cặp Jedi riêng biệt được đặt hàng. Đối với một Jedi được chọn có tổng sức mạnh S, chúng ta có thể cố gắng xác định xem liệu sự phân phối lại nào đó có đánh bại được đối thủ hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa về một cuộc chiến có thể xảy ra. Tuy nhiên, với 500.000 Jedi, có khoảng 2,5⋅10 11 cặp có thứ tự, vượt xa giới hạn thời gian ngay cả khi mỗi so sánh chỉ thực hiện một vài thao tác nguyên thủy. 

Quan sát hữu ích là chúng ta không thực sự cần xem xét chi tiết cả ba tọa độ của đối thủ. 

Giả sử đối phương có tọa độ x,y,z. Muốn đánh bại chúng ở hai tọa độ thì nên chọn hai tọa độ đối thủ nhỏ nhất. Đặt chúng là p<q. Để đánh bại chúng đòi hỏi ít nhất công suất p+1 ở một tọa độ và công suất q+1 ở tọa độ khác, vì vậy tổng công suất tối thiểu cần có là 

p+q+2. 

Nếu Jedi đã được sửa đổi có tổng sức mạnh S, thì đối thủ sẽ có thể bị đánh bại chính xác khi 

S ≥p+q+2. 

Không có lý do gì để tiêu hao năng lượng ở tọa độ thứ ba, vì vậy mọi sức mạnh còn lại có thể được đặt ở đó. Vì p và q là hai tọa độ nhỏ nhất nên mọi cặp tọa độ đối phương khác đều cần ít nhất một công suất bằng nhau. 

Điều này làm giảm mọi đối thủ xuống một giá trị duy nhất, 

k=p+q, 

tổng của hai tham số nhỏ nhất của nó. Một Jedi với tổng sức mạnh S có thể đánh bại chính xác những đối thủ đó bằng 

k<S−2. 

Bây giờ tất cả các đối thủ có thể được đại diện bởi khóa k của họ. Sắp xếp tất cả các phím một lần. Với mỗi Jedi, tìm kiếm nhị phân số lượng khóa nhiều nhất là S−2. Nếu khóa riêng của Jedi cũng thỏa mãn điều kiện, hãy trừ đi một vì bài toán chỉ yêu cầu các Jedi khác. 

Brute-force hoạt động vì mọi cặp đều có thể được kiểm tra độc lập nhưng không thành công vì có quá nhiều cặp. Việc quan sát thấy mọi đối thủ đều có một thống kê đủ duy nhất, tổng của hai tọa độ nhỏ nhất của nó, biến vấn đề thành việc đếm ngưỡng ngoại tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 ) | O(n) | Quá chậm | 
| Tối ưu | O(nlogn) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng Jedi và tính tổng công suất của nó S=a+b+c. Đồng thời tìm hai tọa độ nhỏ nhất và lưu tổng k của chúng. Sau này chúng ta cần cả hai giá trị: S trở thành ngưỡng truy vấn, trong khi k mô tả mức độ khó để đánh bại Jedi này. 
2. Đặt tất cả giá trị k vào một mảng và sắp xếp nó. Sau khi sắp xếp, tất cả các đối thủ có thể bị Jedi đánh bại với ngưỡng T sẽ tạo thành tiền tố của mảng này. 
3. Với mỗi Jedi, hãy tính T=S−2. Giá trị 2 xuất hiện vì việc đánh bại hai tọa độ p và q của đối thủ đòi hỏi phải có p+1 và q+1, mang lại tổng sức mạnh cho p+q+2. 
4. Sử dụng`bisect_right`để đếm xem có bao nhiêu khóa được sắp xếp thỏa mãn k<T.`bisect_right`là cần thiết hơn là`bisect_left`bởi vì sự bình đẳng là hợp lệ. Nếu S=p+q+2, Jedi đã sửa đổi có thể sử dụng chính xác p+1 và q+1 trong hai tọa độ chiến thắng. 
5. Kiểm tra xem khóa riêng của Jedi hiện tại có thỏa mãn k<S−2 hay không. Nếu đúng như vậy, tìm kiếm nhị phân đã bao gồm Jedi đó, vì vậy hãy trừ đi một. Việc tự kiểm tra dựa trên điều kiện giống hệt như mọi Jedi khác, điều này giúp việc hiệu chỉnh trở nên đơn giản ngay cả khi nhiều Jedi có các thông số giống hệt nhau. 
6. Lưu trữ kết quả theo thứ tự đầu vào ban đầu và in tất cả các câu trả lời. 

### Tại sao nó hoạt động 

Đối với đối thủ có tọa độ được sắp xếp p<q<r, mọi lần phân phối lại chiến thắng đều phải vượt quá ít nhất hai trong số các giá trị này. Cặp rẻ nhất có thể vượt quá là p,q, yêu cầu tổng công suất chính xác là p+1+q+1=p+q+2. Nếu tổng của kẻ tấn công ít nhất là số tiền này, thì số tiền đó có thể được gán cho hai tọa độ đó và tất cả sức mạnh còn lại có thể được gán cho tọa độ thứ ba, do đó tồn tại sự phân phối lại hợp lệ. Do đó, đối thủ có thể bị đánh bại chính xác khi khóa p+q của đối thủ đó nhiều nhất là S−2. Việc sắp xếp các khóa này và đếm tiền tố sẽ đưa ra chính xác tất cả các đối thủ có thể đánh bại và trừ đi Jedi hiện tại khi cần thiết sẽ loại bỏ thành viên không hợp lệ duy nhất của tiền tố đó. 

## Giải pháp Python```

```Vòng lặp đầu vào tính toán hai mẩu thông tin cho mỗi Jedi.`total`là lượng sức mạnh cố định có sẵn cho mỗi cuộc chiến trong tương lai. Hai tọa độ nhỏ nhất được tìm thấy mà không cần sắp xếp toàn bộ bộ ba, sử dụng giá trị tối thiểu, tối đa và thực tế là tổng của chúng là`total - min - max`. Điều này giúp quá trình triển khai diễn ra ở quy mô nhỏ và tránh tạo danh sách tạm thời cho mọi Jedi. 

các`keys`mảng chứa các giá trị độ khó của đối thủ p+q. Sắp xếp nó một lần là thao tác tiền xử lý trung tâm từ hướng dẫn. 

Đối với một Jedi có tổng S, ngưỡng là`S - 2`.`bisect_right`trả về vị trí đầu tiên sau tất cả các giá trị bằng hoặc thấp hơn ngưỡng đó, do đó chỉ số trả về của nó chính xác là số lượng mong muốn. 

Điều kiện tự loại bỏ phải sử dụng`<=`, phù hợp với điều kiện tìm kiếm nhị phân. Nếu như`keys[i] == threshold`, Jedi có thể đánh bại đối thủ chính xác ở ranh giới và mục nhập của chính nó cũng phải bị loại bỏ. Không có vấn đề tràn số nguyên trong Python, mặc dù tổng số tối đa là 3⋅10 9. 

Các câu trả lời được viết theo thứ tự đầu vào vì`totals`Và`keys`bảo tồn các chỉ số Jedi ban đầu. Vấn đề chỉ có một ca kiểm thử nên không cần vòng lặp ca kiểm thử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn Jedi tạo ra tổng số và độ khó sau đây. Chìa khóa là tổng của hai tọa độ nhỏ nhất. 

| Jedi | Thông số | Tổng S | Phím k | Ngưỡng S−2 | Khóa được sắp xếp ≤ ngưỡng | Tự bao gồm? | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,3,4) | 8 | 4 | 6 | 1 | Có | 1 | 
| 2 | (2,5,9) | 16 | 7 | 14 | 4 | Có | 3 | 
| 3 | (6,10,3) | 19 | 9 | 17 | 4 | Có | 3 | 
| 4 | (5,2,3) | 10 | 5 | 8 | 3 | Có | 2 | 

Các phím được sắp xếp là`[4, 5, 7, 9]`. Đối với Jedi 1, chỉ có chìa khóa`4`nhiều nhất là`6`, và chìa khóa đó thuộc về chính Jedi 1, vì vậy câu trả lời là 0 trong phép tính này. Nhưng mấu chốt của Jedi 4 là`5`, đó cũng là nhiều nhất`6`, đưa ra hai mục đủ điều kiện, bao gồm cả chính nó. Vì vậy, câu trả lời thực tế là`1`. Cái bàn`Sorted keys`do đó, cột phải được đọc dưới dạng độ dài tiền tố chứ không phải số lượng giá trị khóa riêng biệt. Đối với Jedi 1 tiền tố chứa các khóa`4, 5`, do đó việc trừ chính nó sẽ cho`1`. 

Đầu ra cuối cùng là`1 3 3 2`, phù hợp với mẫu 

### Trường hợp ranh giới 

Hãy xem xét```

```Trạng thái tương ứng là: 

| Jedi | Tổng S | Tổng hai nhỏ nhất k | Ngưỡng S−2 | Chìa khóa đủ điều kiện | Tự xóa | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 0, 0 | 1 | 1 | 
| 2 | 2 | 1 | 0 | 0 | 0 | 1 | 
| 3 | 1 | 0 | -1 | không | 0 | 0 | 

Đối với Jedi 1, cả hai phím còn lại nhiều nhất là`1`, vì vậy nó có thể đánh bại cả hai đối thủ. Đối với Jedi 2 thì chỉ có Jedi 3 mới có key`0`, vậy có đúng một đối thủ có thể bị đánh bại. Jedi 3 chỉ có một đơn vị tổng sức mạnh, không đủ để vượt qua dù chỉ hai tọa độ nhỏ nhất của bất kỳ đối thủ nào ở hai vị trí. 

Ví dụ này cũng chứng minh tại sao tọa độ có giá trị bằng 0 phải được xử lý một cách tự nhiên bằng công thức cặp tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nlogn) | Việc tính toán tất cả các khóa mất O(n), sắp xếp mất O(nlogn) và n tìm kiếm nhị phân lấy tổng O(nlogn). | 
| Không gian | O(n) | Tổng, khóa, khóa được sắp xếp và câu trả lời đều chứa n số nguyên. | 

Với n=500000, việc liệt kê cặp bậc hai là không thể, trong khi các hoạt động sắp xếp nlogn và tìm kiếm nhị phân là thực tế. Thuật toán chỉ lưu trữ một số mảng có kích thước n không đổi, phù hợp với giới hạn bộ nhớ 256 MB với biểu diễn Python nhỏ gọn này. 

## Trường hợp thử nghiệm```

```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0 0`|`0`| Đầu vào có kích thước tối thiểu và tự loại trừ | 
| Ba bản sao của`1 1 1`|`0 0 0`| Giá trị bằng nhau và khóa trùng lặp | 
| Hai bản sao của`1 1 2`|`1 1`| Bao gồm ranh giới tìm kiếm nhị phân | 
|`3 0 0`,`0 1 1`,`0 0 1`|`2 1 0`| Tọa độ 0 và tổng bất đối xứng | 
| 500.000 bản`0 0 0`| 500.000 số 0 | Hành vi n, hiệu suất và bộ nhớ tối đa | 

## Vỏ cạnh 

### Một Jedi duy nhất 

cho```

```tổng là 0, khóa là 0 và ngưỡng là −2. Tìm kiếm nhị phân không tìm thấy khóa nào nhiều nhất là −2, vì vậy câu trả lời là`0`. Ngay cả khi ngưỡng đã cho phép tự khóa, việc tự loại bỏ rõ ràng sẽ khiến Jedi không được tính. 

### Bình đẳng ở ngưỡng cửa 

cho```

```mỗi Jedi có tổng cộng 4, khóa 2 và ngưỡng 2. Các khóa được sắp xếp là`[2,2]`, Vì thế`bisect_right`trả lại`2`. Jedi hiện tại thỏa mãn`2 <= 2`, vì vậy một cái bị loại bỏ và câu trả lời là`1`. 

Phân phối lại thực tế là (2,2,0). Nó hoàn toàn lớn hơn (1,1,2) ở hai tọa độ đầu tiên và tổng của nó vẫn bằng 4. Điều này chứng tỏ rằng đẳng thức trong điều kiện biến đổi phải được chấp nhận. 

### Tọa độ 0 

cho```

```chìa khóa là`0`,`1`, Và`0`, trong khi tổng số là`3`,`2`, Và`1`. 

Đối với Jedi đầu tiên, ngưỡng là`1`, vì vậy cả ba khóa đều đủ điều kiện. Loại bỏ chính nó để lại hai đối thủ. Đối với Jedi thứ hai, ngưỡng là`0`, vì vậy chỉ có Jedi thứ ba đủ điều kiện. Đối với Jedi thứ ba, ngưỡng là`-1`, nên không ai đủ điều kiện. Kết quả đầu ra là`2 1 0`. 

Phép tính không bao giờ giả sử tọa độ dương, vì vậy số 0 không yêu cầu trường hợp đặc biệt. 

### Đối thủ trùng lặp 

Giả sử một số Jedi có các thông số giống hệt nhau. Chìa khóa của họ giống hệt nhau và`bisect_right`cố tình đếm từng bản sao. Việc tự loại bỏ chỉ trừ đi một lần xuất hiện của Jedi hiện tại. 

Ví dụ,```

```có tổng số 3, khóa 2 và ngưỡng 1 cho mỗi Jedi. Không có khóa nào đủ tiêu chuẩn nên tất cả câu trả lời đều bằng 0. Thay vào đó, nếu các tham số chung là (1,1,2), thì mọi khóa sẽ là 2 và mọi ngưỡng cũng sẽ là 2, do đó mỗi Jedi sẽ đếm hai khóa còn lại và tạo ra`2 2 2`. 

### Giá trị tham số rất lớn 

Một Jedi có thể có các tham số như```

```cho tổng 3⋅10 9. Ngưỡng chuyển đổi là 2.999.999.998, vẫn được xử lý chính xác bằng các số nguyên có độ chính xác tùy ý của Python. Không có số học dấu phẩy động nào được sử dụng ở bất kỳ đâu nên việc so sánh vẫn chính xác.
