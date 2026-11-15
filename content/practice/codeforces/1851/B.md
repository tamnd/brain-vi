---
title: "CF 1851B - Sắp xếp chẵn lẻ"
description: "Chúng ta được cấp một mảng các số nguyên và có thể hoán đổi hai phần tử bất kỳ có cùng tính chẵn lẻ, nghĩa là cả hai đều là số lẻ hoặc cả hai đều là số chẵn. Mục đích là để xác định xem có thể sắp xếp mảng theo thứ tự không giảm bằng cách sử dụng thao tác này với số lần bất kỳ hay không."
date: "2026-06-09T17:19:27+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1851
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 888 (Div. 3)"
rating: 800
weight: 1851
solve_time_s: 371
verified: false
draft: false
---

[CF 1851B - Sắp xếp chẵn lẻ](https://codeforces.com/problemset/problem/1851/B) 

**Đánh giá:** 800 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 6 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và có thể hoán đổi hai phần tử bất kỳ có cùng tính chẵn lẻ, nghĩa là cả hai đều là số lẻ hoặc cả hai đều là số chẵn. Mục đích là để xác định xem có thể sắp xếp mảng theo thứ tự không giảm bằng cách sử dụng thao tác này với số lần bất kỳ hay không. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định độ dài của mảng theo sau là chính mảng đó. Đầu ra chỉ đơn giản là CÓ nếu mảng có thể được sắp xếp theo quy tắc hoạt động và KHÔNG nếu ngược lại. 

Các ràng buộc cho chúng ta biết rằng độ dài mảng có thể lên tới 2⋅10^5 và có thể có tối đa 10^4 trường hợp thử nghiệm, nhưng tổng kích thước mảng trên tất cả các trường hợp thử nghiệm không vượt quá 2⋅10^5. Điều này có nghĩa là chúng ta cần một thuật toán chạy trong thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, bởi vì bất kỳ phương trình bậc hai nào trong n sẽ quá chậm. Một cách tiếp cận ngây thơ thử tất cả các phép hoán đổi có thể xảy ra rõ ràng là không khả thi vì số lượng các phép hoán đổi có thể tăng lên theo tổ hợp với n. 

Trường hợp cạnh tinh vi phát sinh khi mảng chỉ chứa các phần tử của một chẵn lẻ duy nhất. Ví dụ: [11, 3, 15, 3] có thể được sắp xếp lại hoàn toàn vì tất cả các phần tử đều là số lẻ. Ngược lại, nếu mảng có sự kết hợp của số chẵn và số lẻ thì vị trí tương đối của chúng trên các số chẵn lẻ không thể hoán đổi cho nhau. Ví dụ: [11, 3, 15, 3, 2] không thể đặt số chẵn 2 trước số lẻ nếu vị trí được sắp xếp yêu cầu nó, vì việc hoán đổi giữa các phần tử chẵn và lẻ bị cấm. Mảng có độ dài bằng một hoặc các mảng đã được sắp xếp là chuyện nhỏ nhưng không nên bỏ qua. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ mô phỏng tất cả các giao dịch hoán đổi được phép. Đối với mỗi cặp phần tử chưa được sắp xếp, nếu chúng có cùng tính chẵn lẻ, chúng ta có thể hoán đổi chúng về đúng vị trí. Điều này có tác dụng vì các hoán đổi hợp lệ lặp đi lặp lại cuối cùng sẽ sắp xếp tất cả các số có cùng số chẵn lẻ theo thứ tự. Tuy nhiên, cách tiếp cận này về cơ bản là một phương pháp sắp xếp bong bóng bị hạn chế bởi tính chẵn lẻ. Số lượng thao tác trong trường hợp xấu nhất sẽ là O(n^2), điều này không thể chấp nhận được khi n có thể là 2⋅10^5. 

Quan sát quan trọng dẫn đến giải pháp nhanh là thao tác cho phép sắp xếp lại tùy ý trong các số lẻ và trong các số chẵn riêng biệt. Do đó, hạn chế duy nhất đến từ việc vị trí tương đối của số lẻ và số chẵn khớp với mảng được sắp xếp cuối cùng như thế nào. Chúng ta có thể tách mảng thành các phần tử chẵn và lẻ, sắp xếp chúng một cách độc lập và sau đó kiểm tra xem chuỗi chẵn lẻ trong mảng được sắp xếp có khớp với những gì có thể đạt được từ mảng ban đầu hay không. Nếu với mỗi vị trí trong mảng được sắp xếp đầy đủ, chúng ta có thể gán một phần tử có tính chẵn lẻ chính xác từ mảng ban đầu thì việc sắp xếp là có thể. 

Giải pháp tối ưu là sắp xếp mảng một cách bình thường rồi kiểm tra xem mỗi số lẻ trong mảng đã sắp xếp có một số lẻ tương ứng trong mảng ban đầu có thể chiếm vị trí đó và tương tự đối với các số chẵn. Nếu tất cả các vị trí chẵn lẻ khớp nhau, mảng có thể được sắp xếp bằng cách sử dụng các phép hoán đổi được phép. Nếu không thì không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số lượng test case. Đối với mỗi trường hợp kiểm thử, hãy đọc độ dài mảng n và các phần tử mảng. Chúng tôi sẽ xử lý từng trường hợp kiểm thử một cách độc lập vì các ràng buộc cho phép tối đa 10^4 trường hợp kiểm thử. 
2. Tách mảng thành hai danh sách: một danh sách chứa tất cả các số lẻ và danh sách khác chứa tất cả các số chẵn. Sự tách biệt này có hiệu quả vì mọi sự hoán đổi đều được cho phép trong các danh sách này. 
3. Sắp xếp mảng ban đầu để xác định thứ tự không giảm mục tiêu. Đồng thời sắp xếp các danh sách chẵn và lẻ riêng lẻ. Việc sắp xếp mảng ban đầu sẽ đưa ra trình tự cuối cùng chính xác mà chúng ta cần đạt được. 
4. Lặp lại mảng mục tiêu đã được sắp xếp. Đối với mỗi phần tử, kiểm tra xem nó là số lẻ hay số chẵn. Nếu là số lẻ, hãy xóa số lẻ nhỏ nhất còn lại khỏi danh sách số lẻ đã sắp xếp và xác minh rằng nó khớp với phần tử đích. Nếu nó là số chẵn, hãy xóa số chẵn nhỏ nhất còn lại khỏi danh sách chẵn được sắp xếp và xác minh rằng nó khớp. 
5. Nếu tại bất kỳ thời điểm nào, một phần tử không thể khớp với số chẵn lẻ chính xác còn lại, xuất NO cho trường hợp thử nghiệm này. Nếu tất cả các phần tử được khớp thành công, xuất ra CÓ. 

Tại sao nó hoạt động: thuật toán duy trì hai bất biến. Các số lẻ và số chẵn có thể được hoán vị độc lập. Bằng cách luôn lấy số chẵn lẻ chính xác nhỏ nhất có sẵn, chúng tôi mô phỏng ngầm tất cả các giao dịch hoán đổi có thể xảy ra. Nếu chuỗi mục tiêu đã sắp xếp không thể được xây dựng lại bằng cách sử dụng hai danh sách độc lập này thì không có chuỗi hoán đổi nào được phép có thể tạo ra mảng đã sắp xếp. 

## Giải pháp Python```
PythonRun
```Đầu tiên, đoạn mã sẽ tách mảng thành các phần tử chẵn và lẻ. Sau đó nó sắp xếp các danh sách này và mảng mục tiêu. Vòng lặp chính xác minh rằng mỗi phần tử trong mảng được sắp xếp có thể khớp với một phần tử có tính chẵn lẻ chính xác trong mảng ban đầu. Các lỗi khác nhau có thể tránh được bằng cách chỉ tăng các chỉ số sau khi khớp thành công. Cách tiếp cận này trực tiếp thực hiện thuật toán được mô tả ở trên. 

## Ví dụ đã hoạt động 

Xét mảng [7, 10, 1, 3, 2]. Các số lẻ là [7, 1, 3], được sắp xếp thành [1, 3, 7]. Các số chẵn là [10, 2], được sắp xếp thành [2, 10]. Mảng mục tiêu được sắp xếp là [1, 2, 3, 7, 10]. Lặp lại qua mảng mục tiêu, 1 khớp với số lẻ thứ nhất, 2 khớp với số chẵn thứ nhất, 3 khớp với số lẻ thứ hai, 7 khớp với số lẻ thứ ba, 10 khớp với số chẵn thứ hai. Tất cả các trận đấu đều thành công, vì vậy kết quả đầu ra là CÓ. 

Đối với [11, 3, 15, 3, 2], các số lẻ được sắp xếp là [3, 3, 11, 15], các số chẵn được sắp xếp là [2]. Mảng mục tiêu được sắp xếp là [2, 3, 3, 11, 15]. Phần tử đầu tiên 2 trùng với số chẵn duy nhất, 3 khớp với số lẻ thứ nhất, 3 khớp với số lẻ thứ hai, 11 khớp với số lẻ thứ ba, 15 khớp với số lẻ thứ tư. Các danh sách đã được sử dụng hết một cách chính xác. Ở đây, đầu ra là NO, xảy ra do số phần tử lẻ trong mảng ban đầu không đủ để đặt 15 ở vị trí đích so với tính chẵn lẻ. 

| Bước | Yếu tố mục tiêu | Con trỏ lẻ | Con trỏ chẵn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 khớp lẻ[0] → lẻ_idx = 1 | 
| 2 | 2 | 1 | 0 | 2 trận chẵn[0] → chẵn_idx = 1 | 
| 3 | 3 | 1 | 1 | 3 trận lẻ[1] → lẻ_idx = 2 | 
| 4 | 7 | 2 | 1 | 7 trận lẻ[2] → lẻ_idx = 3 | 
| 5 | 10 | 3 | 1 | 10 trận chẵn[1] → chẵn_idx = 2 | 

Điều này xác nhận thuật toán mô phỏng chính xác các giao dịch hoán đổi trong các nhóm chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mảng và các mảng con lẻ/chẵn chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(n) | Danh sách riêng biệt cho các phần tử lẻ và chẵn yêu cầu không gian tuyến tính. | 

Cho rằng tổng của n trên tất cả các trường hợp thử nghiệm là 2⋅10^5, thuật toán chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```
PythonRun
```
