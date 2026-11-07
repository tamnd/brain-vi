---
title: "CF 1312B - Bogosort"
description: "Chúng ta được cho một mảng các số nguyên và được phép hoán vị nó một cách tùy ý. Sau khi sắp xếp lại, chúng ta gán từng giá trị cho một vị trí bắt đầu từ 1. Mảng được coi là hợp lệ nếu không có hai vị trí nào có cùng giá trị của biểu thức i - a[i]."
date: "2026-06-16T06:49:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1312
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 83 (Rated for Div. 2)"
rating: 1000
weight: 1312
solve_time_s: 287
verified: false
draft: false
---

[CF 1312B - Bogosort](https://codeforces.com/problemset/problem/1312/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 4 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và được phép hoán vị nó một cách tùy ý. Sau khi sắp xếp lại, chúng ta gán từng giá trị cho một vị trí bắt đầu từ 1. Mảng được coi là hợp lệ nếu không có hai vị trí nào có cùng giá trị của biểu thức`i - a[i]`. Nói cách khác, nếu chúng ta xem mỗi phần tử như xác định một “chỉ số đường chéo” được tính từ vị trí của nó trừ đi giá trị của nó thì tất cả các chỉ số đường chéo này phải khác biệt. 

Nhiệm vụ là quyết định cách sắp xếp lại mảng sao cho điều kiện duy nhất này được giữ nguyên. Vấn đề đảm bảo rằng luôn tồn tại ít nhất một thứ tự hợp lệ. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi mảng có độ dài tối đa là 100 và các giá trị cũng bị giới hạn bởi 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào có hành vi bậc hai cho mỗi trường hợp thử nghiệm đều dễ dàng đủ nhanh. Ngay cả cách tiếp cận bậc ba vẫn phù hợp trong trường hợp xấu nhất, nhưng bất cứ điều gì liên quan đến tìm kiếm theo cấp số nhân hoặc quay lui các hoán vị sẽ không cần thiết. 

Một điểm tinh tế là điều kiện phụ thuộc vào cả chỉ số và giá trị. Một cách giải thích ngây thơ có thể cho rằng các giá trị bằng nhau là có vấn đề, nhưng thực tế không phải vậy. Hai giá trị giống nhau đều được miễn là chúng được đặt ở các vị trí tạo ra các giá trị khác nhau`i - a[i]`các giá trị. 

Một trường hợp thất bại vì suy nghĩ ngây thơ sẽ khiến mảng không thay đổi. Ví dụ, nếu chúng ta lấy`[1, 1, 3, 5]`như đã cho, thứ tự ban đầu có thể vi phạm điều kiện vì các bản sao có thể căn chỉnh theo cách tạo ra sự lặp lại`i - a[i]`. Một trường hợp sai lầm khác là cho rằng việc sắp xếp luôn có hiệu quả. Sắp xếp`[3, 2, 1]`cho`[1, 2, 3]`, nhưng điều này không đảm bảo sự khác biệt`i - a[i]`giá trị nói chung, do đó cần phải có cách xây dựng khác. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chúng ta không cần phải suy luận về các giá trị tương tác trực tiếp với nhau. Điều kiện chỉ hạn chế sự khác biệt giữa chỉ số và giá trị. Nếu chúng ta muốn tất cả`i - a[i]`để khác biệt, chúng ta có thể kiểm soát nó bằng cách tách biệt cẩn thận các giá trị lớn khỏi các giá trị nhỏ. 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của mảng và kiểm tra điều kiện cho từng hoán vị. Điều này đúng vì nó tìm kiếm toàn diện tất cả các sắp xếp lại hợp lệ. Tuy nhiên, số lượng hoán vị tăng lên khi`n!`, cái nào cho`n = 100`có kích thước lớn về mặt thiên văn. Ngay cả đối với`n = 10`, nó trở thành ranh giới trong thực tế, vì vậy cách tiếp cận này hoàn toàn không khả thi. 

Hiểu biết sâu sắc về cấu trúc là chúng ta có thể tránh xung đột bằng cách đảm bảo rằng các giá trị lớn hơn được đặt ở các vị trí sớm hơn và các giá trị nhỏ hơn được đặt sau. Nếu chúng ta sắp xếp mảng theo thứ tự giảm dần thì gán nó theo thứ tự đó, biểu thức`i - a[i]`cũng có xu hướng giảm, nhưng quan trọng hơn, không có hai phần tử nào có thể tạo ra sự khác biệt như nhau vì các giá trị bằng nhau được tách biệt và chỉ số của chúng tăng dần. 

Lý do sâu xa hơn mà điều này có tác dụng là nếu các giá trị được sắp xếp giảm dần thì sự khác biệt giữa các phần tử liền kề trong chuỗi được xây dựng không thể chuyển thành bằng nhau trừ khi cả sự thay đổi chỉ số và giá trị đều căn chỉnh hoàn hảo, điều này không thể xảy ra khi sắp xếp các giá trị theo thứ tự nghiêm ngặt dưới các bản sao tùy ý. 

Điều này làm giảm vấn đề từ việc tìm kiếm hoán vị xuống chỉ còn một bước sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp giảm dần | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng cho test case hiện tại. Mục đích là xây dựng bất kỳ hoán vị nào tránh được các giá trị lặp lại của`i - a[i]`. 
2. Sắp xếp mảng theo thứ tự giảm dần. Điều này đảm bảo rằng các giá trị lớn hơn được chỉ định các vị trí sớm hơn trong cách sắp xếp cuối cùng, giúp dàn trải các chênh lệch được tính toán. 
3. Xuất ra mảng đã sắp xếp dưới dạng sắp xếp cuối cùng. Không cần chuyển đổi thêm nữa vì thứ tự sắp xếp đã thỏa mãn điều kiện. 

### Tại sao nó hoạt động 

Khi mảng được sắp xếp theo thứ tự không tăng, các phần tử sẽ được sắp xếp sao cho các giá trị lớn hơn xuất hiện ở các chỉ số nhỏ hơn. Nếu hai vị trí`i < j`có cùng giá trị của`i - a[i] = j - a[j]`, sau đó sắp xếp lại sẽ cho`a[i] - a[j] = i - j`. Từ`i - j`là âm, điều này đòi hỏi`a[i] < a[j]`, mâu thuẫn với thực tế là mảng không tăng. Do đó không có sự đẳng thức nào có thể xảy ra và mọi giá trị của`i - a[i]`phải khác biệt. 

## Giải pháp Python```
PythonRun
```Việc thực hiện là trực tiếp. Mỗi trường hợp thử nghiệm được xử lý độc lập. Sắp xếp theo thứ tự ngược lại là phép biến đổi duy nhất được áp dụng. Đầu ra được in ngay lập tức. 

Một sai lầm phổ biến là quên rằng việc sắp xếp phải giảm dần chứ không phải tăng dần. Thứ tự tăng dần có thể tạo ra xung đột trong`i - a[i]`biểu thức khi các giá trị nhỏ tích lũy sớm trong mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, 1, 3, 5]`Sắp xếp giảm dần:`[5, 3, 1, 1]`| tôi | một [tôi] | tôi - một[i] | 
| --- | --- | --- | 
| 1 | 5 | -4 | 
| 2 | 3 | -1 | 
| 3 | 1 | 2 | 
| 4 | 1 | 3 | 

Tất cả các giá trị đều khác biệt nên sự sắp xếp là hợp lệ. 

Dấu vết này cho thấy các bản sao không gây trở ngại như thế nào vì chúng được đặt ở các chỉ số khác nhau tạo ra những khác biệt khác nhau. 

### Ví dụ 2 

Mảng đầu vào:`[3, 2, 1, 5, 6, 4]`Sắp xếp giảm dần:`[6, 5, 4, 3, 2, 1]`| tôi | một [tôi] | tôi - một[i] | 
| --- | --- | --- | 
| 1 | 6 | -5 | 
| 2 | 5 | -3 | 
| 3 | 4 | -1 | 
| 4 | 3 | 1 | 
| 5 | 2 | 3 | 
| 6 | 1 | 5 | 

Một lần nữa, tất cả các giá trị đều khác biệt. Cấu trúc của các giá trị giảm nghiêm ngặt đảm bảo một chuỗi sai phân tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ cho mảng | 

Các ràng buộc cho phép tối đa 100 phần tử cho mỗi trường hợp thử nghiệm, do đó việc sắp xếp diễn ra tức thời một cách hiệu quả. Ngay cả với 100 trường hợp thử nghiệm, tổng công việc là không đáng kể. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | nhiều bộ không thay đổi | trùng lặp xử lý an toàn | 
| đầu vào tăng dần | đảo ngược hoàn toàn | hiệu ứng phân loại | 
| trùng lặp hỗn hợp | đặt hàng nhiều bộ ổn định | độ bền trường hợp chung | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống nhau thì bất kỳ hoán vị nào cũng tạo ra các giá trị giống nhau của`i - a[i]`chỉ khi các chỉ số trùng nhau, nhưng chúng không trùng nhau, vì vậy điều kiện vẫn hợp lệ. Việc sắp xếp giữ cho mảng không thay đổi và kết quả rất chính xác. 

Khi mảng đã tăng lên, việc đảo ngược nó sẽ tạo ra một chuỗi giảm dần, giúp tối đa hóa sự phân tách giữa các giá trị và ngăn chặn bất kỳ sự bằng nhau nào của`i - a[i]`. 

Khi các bản sao tồn tại ở các vị trí không cần thiết, việc xây dựng đảm bảo chúng được trải rộng trên các chỉ số khác nhau. Mặc dù các giá trị lặp lại, các chỉ số đủ khác nhau để sự khác biệt được tính toán không thể khớp giữa hai vị trí theo cách vi phạm thứ tự.
