---
title: "CF 1539D - Giá cố định"
description: "Chúng tôi được yêu cầu xác định tổng chi phí tối thiểu để Lena mua một bộ sản phẩm trong đó mỗi sản phẩm có số lượng yêu cầu và ngưỡng giảm giá."
date: "2026-06-10T14:41:47+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1539
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 727 (Div. 2)"
rating: 1600
weight: 1539
solve_time_s: 268
verified: true
draft: false
---

[CF 1539D - Đã sửa giá](https://codeforces.com/problemset/problem/1539/D) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam, triển khai, sắp xếp, hai con trỏ 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định tổng chi phí tối thiểu để Lena mua một bộ sản phẩm trong đó mỗi sản phẩm có số lượng yêu cầu và ngưỡng giảm giá. Mỗi sản phẩm ban đầu có giá 2 rúp và sau khi mua ít nhất`b_i`mặt hàng của bất kỳ sản phẩm nào, giá của sản phẩm`i`giảm xuống 1 rúp mỗi đơn vị cho tất cả các lần mua tiếp theo. Lena có thể mua nhiều hơn số lượng yêu cầu nếu điều đó giúp giảm chi phí chung. 

Đầu vào cho chúng ta`n`các sản phẩm. Mỗi sản phẩm đều có yêu cầu`a_i`và ngưỡng giảm giá`b_i`. Sản lượng của chúng tôi là tổng chi phí nhỏ nhất để đáp ứng tất cả các yêu cầu. Tổng của tất cả`a_i`có thể lớn như`10^14`, vì vậy các thuật toán mô phỏng từng giao dịch mua riêng lẻ sẽ quá chậm. Bởi vì`n`có thể lên đến`10^5`, chúng tôi không thể sử dụng các thuật toán có độ phức tạp về thời gian kém hơn O(n log n), nhưng chúng tôi có thể xử lý các đường truyền tuyến tính hoặc gần như tuyến tính thông qua các cấu trúc dữ liệu được sắp xếp hoặc dựa trên mức độ ưu tiên. Các trường hợp khó khăn không rõ ràng bao gồm khi giảm giá rẻ nhất có sẵn muộn, yêu cầu Lena phải mua thêm các mặt hàng của sản phẩm khác trước hoặc khi một số sản phẩm có ngưỡng rất cao so với số lượng yêu cầu. Một cách tiếp cận ngây thơ là mua từng sản phẩm theo thứ tự tùy ý có thể bỏ qua cơ hội được giảm giá và có thể chi tiêu quá mức đáng kể. 

Ví dụ: nếu chúng ta có hai sản phẩm với`a=[1,1]`Và`b=[2,1]`, một cách tiếp cận bất cẩn khi mua chúng một cách tuần tự có thể phải trả giá đầy đủ cho cả hai mặt hàng. Chiến lược tối ưu là mua sản phẩm thứ hai trước để được giảm giá cho sản phẩm đầu tiên, giảm chi phí từ 4 xuống 3 rúp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các đơn đặt hàng có thể có. Đối với mỗi ngày hoặc khi mua hàng, chúng tôi sẽ kiểm tra xem mặt hàng nào có giá phải chăng với mức giá chiết khấu và chọn một mặt hàng, lặp lại cho đến khi đáp ứng tất cả các yêu cầu. Về nguyên tắc, điều này có tác dụng vì mỗi lần mua sẽ giảm chi phí trong tương lai của một số mặt hàng, nhưng số lượng hoán vị tăng theo giai thừa với`n`và số lượng`a_i`là rất lớn, lên tới`10^14`. Việc mô phỏng từng giao dịch mua riêng lẻ là không khả thi vì ngay cả một sản phẩm có`a_i = 10^{14}`sẽ yêu cầu`10^{14}`lần lặp lại. 

Quan sát quan trọng là thứ tự mua hàng chỉ quan trọng khi có giảm giá và sau khi giảm giá được mở khóa, nó vẫn có sẵn cho tất cả các giao dịch mua trong tương lai. Điều này khiến chúng tôi phải suy nghĩ một cách tham lam: chúng tôi muốn ưu tiên mở khóa giảm giá cho các sản phẩm đắt tiền càng sớm càng tốt. Vì mỗi sản phẩm ban đầu có giá 2 rúp nên chúng ta luôn có thể bắt đầu với những sản phẩm có giá trị nhỏ nhất`b_i`(ngưỡng) để nhanh chóng mở khóa các khoản giảm giá khác. Tuy nhiên, số lượng`a_i`lớn nên chúng ta không thể tham lam mua từng đơn vị. Thay vào đó, chúng ta có thể sử dụng cách tiếp cận hai con trỏ: một con trỏ dành cho các mặt hàng "mua với giá đầy đủ" rẻ nhất và một con trỏ khác dành cho các mặt hàng đã được giảm giá hoặc đã đạt đến ngưỡng. Chúng tôi sắp xếp sản phẩm theo`b_i`giá trị, sau đó mô phỏng việc mua hàng loạt bằng cách sử dụng hai con trỏ để tối đa hóa số lượng mua hàng giảm giá trong khi đáp ứng tất cả`a_i`yêu cầu. Điều này làm giảm độ phức tạp thành tuyến tính về số lượng sản phẩm sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng a_i) | O(n) | Quá chậm đối với đầu vào lớn | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các yêu cầu về sản phẩm`a_i`và ngưỡng chiết khấu`b_i`. Lưu trữ chúng theo cặp`(b_i, a_i)`. Chúng tôi sẽ sắp xếp theo`b_i`ưu tiên giảm giá mở khóa sớm. 
2. Sắp xếp sản phẩm theo ngưỡng giảm giá`b_i`theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng tôi mua đủ đơn vị để đạt đến ngưỡng, chúng tôi sẽ được giảm giá cho nhiều sản phẩm nhất có thể với chi phí ban đầu ở mức tối thiểu. 
3. Khởi tạo hai con trỏ,`l`ở đầu danh sách được sắp xếp và`r`ở cuối.`l`đại diện cho sản phẩm tiếp theo được mua ở mức giá chiết khấu hoặc đã mua,`r`đại diện cho sản phẩm tiếp theo dành cho các giao dịch mua nguyên giá để được giảm giá. 
4. Theo dõi số lượng đơn vị đã mua tích lũy cho đến nay (`total_bought`) và tổng chi phí (`cost`). Lặp lại cho đến hết`a_i`được thỏa mãn: 

một. Nếu như`total_bought >= b_i`cho sản phẩm tại con trỏ`l`, chúng ta có thể mua các đơn vị với giá chiết khấu (1 rúp). Mua tất cả các đơn vị còn lại cần thiết và tăng dần`total_bought`Và`cost`. 

b. Nếu không, hãy mua đơn vị nguyên giá (2 rúp) từ sản phẩm tại con trỏ`r`tăng lên`total_bought`đủ để mở khóa giảm giá. Chỉ mua số lượng tối thiểu cần thiết, không nhiều hơn số lượng cần thiết cho sản phẩm đó. 
5. Tiếp tục quá trình này di chuyển con trỏ vào trong (`l`tăng khi mua hàng giảm giá được thực hiện,`r`giảm khi việc mua hàng nguyên giá được thực hiện) cho đến khi tất cả số lượng`a_i`được mua. 

Tính bất biến đó là`total_bought`luôn phản ánh chính xác tổng số mặt hàng đã mua, từ đó xác định sản phẩm nào có thể mua được giảm giá. Thuật toán không bao giờ mua nhiều mặt hàng nguyên giá hơn mức cần thiết để được giảm giá. Sắp xếp theo`b_i`đảm bảo rằng chiết khấu rẻ hơn được sử dụng trước tiên, đảm bảo tổng chi phí tối thiểu. 

## Giải pháp Python```
PythonRun
```Đầu tiên, giải pháp sẽ đọc tất cả thông tin sản phẩm và lưu trữ theo cặp`(b_i, a_i)`để sắp xếp dễ dàng hơn theo ngưỡng giảm giá. Việc sắp xếp đảm bảo rằng các sản phẩm có ngưỡng thấp hơn sẽ được xem xét đầu tiên. Vòng lặp hai con trỏ quản lý việc mua hàng một cách cẩn thận: các giao dịch mua hàng giảm giá được xử lý tại con trỏ`l`và các giao dịch mua với giá đầy đủ được lấy từ con trỏ`r`để mở khóa giảm giá trong tương lai. Chỉ mua số lượng mặt hàng nguyên giá tối thiểu cần thiết để tránh bội chi. Thuật toán kết thúc khi tất cả các đơn vị yêu cầu được mua. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Bước | tôi | r | tổng_mua | hành động | chi phí | còn lại a_i | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 0 | giá đầy đủ 1 từ r=2 | 2 | [3,1,0] | 
| 2 | 0 | 2 | 1 | giá đầy đủ 1 từ r=2 | 4 | [3,1,0] | 
| 3 | 0 | 1 | 2 | giá đầy đủ 1 từ r=1 | 6 | [3,0] | 
| 4 | 0 | 0 | 3 | giảm giá 1 từ l=0 | 7 | [2] | 
| 5 | 0 | 0 | 4 | giảm giá 2 từ l=0 | 8 | [0] | 

Bảng xác nhận rằng tổng chi phí 8 đã đạt được. Tất cả sản phẩm đều đáp ứng đủ số lượng yêu cầu, được áp dụng chiết khấu tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp theo`b_i`thống trị; vòng lặp hai con trỏ là O(n) | 
| Không gian | O(n) | Lưu trữ các cặp sản phẩm và con trỏ | 

Giải pháp xử lý hiệu quả tới 100.000 sản phẩm và số lượng rất lớn vì nó tránh lặp lại trên các đơn vị riêng lẻ. Mức sử dụng bộ nhớ tỷ lệ tuyến tính với số lượng sản phẩm, nằm trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```
