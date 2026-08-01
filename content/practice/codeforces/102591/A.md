---
title: "CF 102591A - 3435"
description: "Chúng ta cần đếm các số nguyên bên trong khoảng [l, r] bằng tổng của một giá trị đặc biệt được gán cho mỗi chữ số của chúng. Đối với một chữ số x, đóng góp của nó là x^x, do đó, một số hợp lệ khi cộng tổng đóng góp của tất cả các chữ số của nó sẽ tạo lại số ban đầu."
date: "2026-07-31T15:53:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "A"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 427
verified: true
draft: false
---

[CF 102591A - 3435](https://codeforces.com/problemset/problem/102591/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các số nguyên bên trong một khoảng`[l, r]`bằng tổng của một giá trị đặc biệt được gán cho mỗi chữ số của chúng. Đối với một chữ số`x`, đóng góp của nó là`x^x`, do đó, một số là hợp lệ khi cộng các phần đóng góp của tất cả các chữ số của nó sẽ tạo lại số ban đầu. 

Ví dụ, số`3435`hợp lệ vì các chữ số của nó đóng góp`3^3 + 4^4 + 3^3 + 5^5`, bằng`3435`. 

Các điểm cuối của khoảng có thể lớn bằng`10^9`. Việc quét trực tiếp mọi số sẽ yêu cầu kiểm tra tới một tỷ ứng viên và ngay cả việc kiểm tra chữ số rất nhanh cũng không đủ. Thuộc tính hữu ích là số lượng chữ số nhỏ. Mỗi ứng viên có tối đa mười chữ số nên thay vì tìm kiếm qua số, chúng ta có thể tìm kiếm qua tổ hợp chữ số. 

Các trường hợp đặc biệt chủ yếu xuất phát từ việc coi số đó là một số nguyên thông thường thay vì một chuỗi các chữ số. chữ số`0`có thể xảy ra bên trong một số và sự đóng góp của nó là`0^0`theo định nghĩa thông thường được sử dụng bởi vấn đề này, nó phải được xử lý như`1`bởi vì sự đóng góp của chữ số`0`được định nghĩa là`0^0`trong công thức chữ số. Việc thực hiện bất cẩn khi sử dụng hàm công suất bình thường có thể tạo ra các kết quả khác nhau trong trường hợp này. Ví dụ, khoảng`1 1`chứa số hợp lệ`1`, vì vậy đầu ra đúng là`1`. Việc triển khai bỏ qua trường hợp một chữ số có thể trả về không chính xác`0`. 

Một trường hợp cạnh khác là ranh giới trên. số`438579088`là hợp lệ và ở dưới`10^9`, do đó đầu vào`438579088 438579088`phải quay lại`1`. Một giải pháp chỉ tính toán trước các giá trị dưới giới hạn nhỏ hơn hoặc vô tình sử dụng giới hạn trên nghiêm ngặt sẽ bỏ lỡ giải pháp đó. 

Khoảng thời gian cũng có thể không chứa giá trị hợp lệ. Ví dụ,`2 10`không có số hợp lệ, vì vậy câu trả lời là`0`. Việc triển khai mạnh mẽ chỉ kiểm tra tổng các chữ số đối với các số có nhiều chữ số có thể đếm không chính xác hoặc bỏ qua các giá trị nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp qua mọi số từ`l`ĐẾN`r`, tính tổng các chữ số đóng góp và so sánh với số ban đầu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của một số hợp lệ. Tuy nhiên, khoảng lớn nhất có thể chứa khoảng một tỷ số. Mỗi séc kiểm tra tối đa mười chữ số, đưa ra khoảng`10^10`các phép toán chữ số trong trường hợp xấu nhất, vượt xa những gì thực tế. 

Quan sát quan trọng là có rất ít câu trả lời khả thi. Một số có tối đa mười chữ số chỉ có thể được hình thành từ mười lựa chọn cho mỗi chữ số và số lượng kết hợp hợp lệ là rất nhỏ. Chúng ta có thể đảo ngược hướng tìm kiếm. Thay vì hỏi xem mọi số có hợp lệ hay không, chúng ta tạo ra tất cả các tổng có thể có của các chữ số đóng góp và xây dựng lại số tương ứng. 

Trong quá trình tìm kiếm chữ số, chúng tôi chọn từng vị trí chữ số. Số tiền đóng góp được tích lũy đồng thời với giá trị số thực tế. Sau khi tất cả các vị trí được chọn, nếu số được xây dựng bằng tổng đóng góp tích lũy thì chúng ta đã tìm thấy một số hợp lệ. 

Chỉ có mười một độ dài có thể được xem xét nếu chúng ta bao gồm phạm vi lên đến`10^9`và việc cắt tỉa mạnh mẽ vì sự đóng góp của một chữ số là cố định. Việc tìm kiếm truy cập ít trạng thái hơn nhiều so với việc quét theo khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1) * 10) | O(1) | Quá chậm | 
| Tối ưu | O(10 * 10^10) ở giới hạn trên lỏng lẻo, nhỏ hơn nhiều khi cắt tỉa | O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước phần đóng góp của mỗi chữ số từ`0`ĐẾN`9`. Giá trị chỉ phụ thuộc vào chữ số, vì vậy việc tính toán nó một lần sẽ tránh được việc lặp lại trong quá trình tìm kiếm. 
2. Tạo mọi số có thể có tối đa mười chữ số bằng cách sử dụng tìm kiếm theo chiều sâu. Trong khi xây dựng một số, hãy giữ hai giá trị: số được biểu thị bằng các chữ số đã chọn và tổng các phần đóng góp của các chữ số. 
3. Bỏ qua các số có số 0 đứng đầu bằng cách chỉ bắt đầu độ dài từ một chữ số và chỉ cho phép các số 0 sau chữ số được chọn đầu tiên. Điều này ngăn việc tạo cùng một số nhiều lần với độ dài khác nhau. 
4. Khi một chuỗi chữ số hoàn chỉnh được tạo, hãy so sánh số được xây dựng với tổng đóng góp tích lũy. Nếu chúng bằng nhau và số nằm bên trong`[l, r]`, thêm nó vào câu trả lời. 
5. Sắp xếp hoặc lưu trữ các số hợp lệ được tạo và đếm số lượng rơi vào khoảng được yêu cầu. 

Lý do điều này hoạt động là vì mọi số có thể có trong phạm vi cho phép đều có biểu diễn chính xác một chữ số mà không có số 0 đứng đầu. Việc tìm kiếm liệt kê mọi biểu diễn như vậy, tính toán giá trị chính xác theo yêu cầu của định nghĩa và chỉ chấp nhận các biểu diễn mà hai giá trị khớp nhau. Vì không có biểu diễn hợp lệ nào bị bỏ qua và không có biểu diễn không hợp lệ nào được chấp nhận nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```
Python
```Mảng`pow_digit`lưu trữ thông tin duy nhất cần thiết từ mỗi chữ số. Xử lý chữ số 0 một cách rõ ràng tránh việc dựa vào cách giải thích ngôn ngữ cụ thể của`0 ** 0`. 

Hàm đệ quy giữ`value`, số lượng thực tế hiện đang được xây dựng, và`total`, tổng của các chữ số đóng góp. Khi tất cả các vị trí đã được lấp đầy, đẳng thức giữa hai biến này chính xác là điều kiện của bài toán. 

Việc tìm kiếm xem xét độ dài từ một đến mười vì`10^9`có mười chữ số. Các số 0 đứng đầu bị loại bỏ ở vị trí đầu tiên, điều này tránh được sự biểu diễn trùng lặp. Kiểm tra giới hạn trên cho phép các nhánh lớn hơn`r`phải dừng lại ngay lập tức. 

Số nguyên Python không bị tràn, do đó việc triển khai không cần xử lý đặc biệt đối với các giá trị trung gian lớn. Kiểm tra phạm vi cuối cùng bao gồm cả hai bên, khớp với định nghĩa của khoảng. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```

```tìm kiếm tạo ra các số hợp lệ bên dưới`10^9`, sau đó kiểm tra cái nào thuộc về khoảng. 

| Chiều dài | Ứng viên | Tổng đóng góp chữ số | hợp lệ | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có, phạm vi ngoài | 0 | 
| 4 | 3435 | 3435 | Có, trong phạm vi | 1 | 
| 9 | 438579088 | 438579088 | Có, phạm vi ngoài | 1 | 

Dấu vết cho thấy thuật toán không cần kiểm tra mọi số giữa`2020`Và`4040`. Nó tìm thấy tập hợp nhỏ các số hợp lệ và thực hiện kiểm tra phạm vi sau đó. 

Một ví dụ thứ hai là:```

```| Chiều dài | Ứng viên | Tổng đóng góp chữ số | hợp lệ | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có, trong phạm vi | 1 | 
| 2 | không | không | không | 1 | 

Điều này xác nhận rằng các giá trị một chữ số được bao gồm và các ranh giới khoảng được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (10 * trạng thái được tạo) | Phép đệ quy khám phá các bài tập chữ số, nhưng số lượng ứng viên hoàn thành thực tế là rất nhỏ vì chỉ tồn tại mười vị trí chữ số. | 
| Không gian | O(10) | Độ sâu đệ quy tối đa là 10 và danh sách câu trả lời được lưu trữ chỉ chứa một vài số. | 

Kích thước đầu vào tối đa ngăn cản việc quét toàn bộ khoảng thời gian. Cách tiếp cận tạo chữ số chỉ phụ thuộc vào số chữ số, vẫn nhỏ đối với giới hạn nhất định. 

## Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Xử lý khoảng thời gian nhỏ nhất có thể và các câu trả lời có một chữ số | 
|`2 10`|`0`| Xử lý các khoảng không có giá trị hợp lệ | 
|`3435 3435`|`1`| Kiểm tra bao gồm ranh giới dưới và trên | 
|`438579088 1000000000`|`1`| Kiểm tra phía tối đa của phạm vi đầu vào | 

## Vỏ cạnh 

Đối với khoảng thời gian`1 1`, thuật toán bắt đầu tìm kiếm một chữ số, chọn chữ số`1`và tính toán cả giá trị được xây dựng và tổng đóng góp như`1`. Chúng khớp nhau nên câu trả lời trở thành`1`. Điều này ngăn ngừa mất số có một chữ số hợp lệ. 

Đối với khoảng thời gian`438579088 438579088`, DFS đạt tới biểu diễn chín chữ số`438579088`. Số tiền đóng góp tích lũy cũng được`438579088`, do đó giá trị được tính. Điều này xác nhận rằng phần trên của phạm vi được phép được bao gồm. 

Đối với khoảng thời gian`2 10`, số hợp lệ duy nhất trong khu vực đó sẽ là`1`, nhưng nó nằm ngoài khoảng đó. Danh sách đã tạo được kiểm tra theo cả hai giới hạn, để lại số đếm ở`0`. Điều này xử lý phạm vi kết quả trống một cách chính xác.
