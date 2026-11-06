---
title: "CF 1279E - Hoán vị năm mới"
description: "Chúng ta được cấp một hoán vị của các số từ 1 đến n và một quy trình khá bất thường là chia hoán vị này thành các “khối” liên tiếp dựa trên khả năng tiếp cận thông qua các cạnh chức năng được xác định bởi chính hoán vị đó."
date: "2026-06-16T02:12:53+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1279
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 79 (Rated for Div. 2)"
rating: 2700
weight: 1279
solve_time_s: 464
verified: false
draft: false
---

[CF 1279E - Hoán vị năm mới](https://codeforces.com/problemset/problem/1279/E) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 7 phút 44 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị của các số từ 1 đến n và một quy trình khá bất thường là chia hoán vị này thành các “khối” liên tiếp dựa trên khả năng tiếp cận thông qua các cạnh chức năng được xác định bởi chính hoán vị đó. 

Mỗi chỉ mục trỏ đến một giá trị và nếu chúng ta tiếp tục đi theo các con trỏ này, chúng ta sẽ ở bên trong một cấu trúc được kết nối được hình thành bởi các cạnh định hướng của hoán vị. Mỗi vị trí thuộc về chính xác một thành phần có thể truy cập như vậy và mỗi thành phần sẽ trở thành một khối khi chúng ta quét từ trái sang phải và chọn phần tử chưa được đánh dấu đầu tiên. 

Bên trong mỗi khối, chúng ta liệt kê các phần tử theo thứ tự chúng xuất hiện trong hoán vị ban đầu, sau đó xoay khối sao cho giá trị lớn nhất của nó di chuyển lên phía trước. Sau khi xử lý tất cả các khối, chúng tôi sắp xếp các khối theo phần tử đầu tiên và ghép chúng lại. 

Một hoán vị được gọi là tốt nếu áp dụng toàn bộ quá trình phân rã và tái thiết khối này trả về cùng một hoán vị không thay đổi. Nhiệm vụ là liệt kê tất cả các hoán vị tốt như vậy theo thứ tự từ điển và xuất ra hoán vị thứ k. 

Ràng buộc n 50 đủ nhỏ để không thể liệt kê trực tiếp sự tăng trưởng giai thừa. Ngay cả việc lưu trữ tất cả các hoán vị cũng không thể thực hiện được ngoài n khoảng 12 hoặc 13. Vì k có thể lớn tới 10^18, nên số lượng hoán vị hợp lệ là rất lớn, nhưng vẫn có cấu trúc đủ để cho phép đếm tổ hợp với DP thay vì tạo ra lực lượng vũ phu. 

Một cách tiếp cận đơn giản sẽ mô phỏng tất cả các hoán vị và kiểm tra phép biến đổi, nhưng ngay cả với n = 10 thì con số này cũng đã là 10 rồi! ≈ 3,6 triệu trạng thái và việc kiểm tra cấu trúc trên mỗi hoán vị rất tốn kém. Một trường hợp thất bại tinh vi khác xuất hiện nếu người ta cố gắng tham lam xây dựng các hoán vị mà không hiểu cách các khối tương tác với nhau, vì việc chuyển đổi phụ thuộc vào khả năng tiếp cận toàn cầu chứ không chỉ thứ tự cục bộ. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu được cấu trúc nào tồn tại sau sự biến đổi không thay đổi. 

Sự phân tách được mô tả về cơ bản là trích xuất các thành phần liên thông của biểu đồ hoán vị được hình thành bởi các cạnh i → p[i], nhưng được lấy theo thứ tự xuất hiện trong mảng. Trong mỗi thành phần, phép quay sẽ di chuyển phần tử lớn nhất lên phía trước và sau đó các khối được sắp xếp theo mức tối đa đó. 

Để một hoán vị được cố định bằng phép biến đổi, phải có hai điều kiện đồng thời. Đầu tiên, mọi khối phải có mức tối đa ở vị trí đầu tiên, nếu không bước xoay sẽ thay đổi nó. Thứ hai, các khối phải xuất hiện theo thứ tự tăng dần của các phần tử đầu tiên, nếu không việc sắp xếp sẽ sắp xếp lại chúng. 

Bây giờ hãy quan sát xem khối thực sự là gì. Nếu chúng ta xét một thành phần được hình thành bằng cách tuân theo p[i], thì đây chính xác là một chu trình trong biểu đồ hoán vị. Vì vậy, mỗi khối tương ứng với một chu kỳ, nhưng được viết theo thứ tự do quét mảng tạo ra, không nhất thiết phải theo thứ tự tuần hoàn. 

Bên trong một chu trình, phép biến đổi buộc phần tử lớn nhất của chu trình đó được đặt trước. Để hoán vị không thay đổi, mức tối đa đó phải là phần tử đầu tiên trong chu trình khi nó xuất hiện trong hoán vị. 

Do đó, mọi chu trình phải xuất hiện trong mảng sao cho phần tử lớn nhất của nó là lần xuất hiện đầu tiên của chu trình đó và các chu trình phải được sắp xếp theo thứ tự tăng dần của phần tử lớn nhất. Điều này có nghĩa là hoán vị được xác định hoàn toàn bằng cách phân chia {1..n} thành các chu trình, trong đó mỗi chu kỳ được viết bắt đầu từ mức tối đa của nó và các chu kỳ được nối theo thứ tự tăng dần của mức tối đa của chúng.

Vì vậy, vấn đề giảm xuống còn việc đếm và xây dựng các hoán vị được hình thành bằng cách chia các số thành các chu trình, mỗi chu trình có nghiệm cực đại và sắp xếp các chu trình theo các nghiệm này. Thứ tự bên trong của một chu trình tương ứng với hoán vị tuần hoàn của các phần tử của nó, nhưng được neo ở mức tối đa, nghĩa là tất cả các phần tử khác có thể xuất hiện theo bất kỳ thứ tự nào phù hợp với cấu trúc chu trình. 

Điều này trở thành DP tổ hợp trên các tập hợp con, trong đó chúng tôi chọn kích thước của chu kỳ đầu tiên, chọn các phần tử của nó, gán cho nó một cấu trúc tuần hoàn với mức cố định tối đa đầu tiên và lặp lại. 

Cách tiếp cận vũ phu sẽ thử tất cả các hoán vị và kiểm tra tính hợp lệ, đó là giai thừa. Cách tiếp cận tối ưu liệt kê các phân vùng chu trình hợp lệ bằng cách sử dụng tổ hợp, đếm cho mỗi tập hợp con có bao nhiêu cách để tạo thành một chu trình gốc hợp lệ và nhân qua các khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| DP tối ưu trên các tập hợp con | O(n^2 · 2^n) | O(n · 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng các hoán vị theo thứ tự từ điển, do đó chúng ta xây dựng chúng từ trái sang phải bằng cách quyết định các chu trình theo thứ tự tăng dần của phần tử tối đa của chúng. 

Mỗi số n xác định nó thuộc về chu trình nào và n luôn là số lớn nhất trong chu trình của nó. Điều này tuân theo ràng buộc về cấu trúc rằng các chu trình được sắp xếp theo mức tối đa của chúng, do đó chu trình cuối cùng phải chứa n. 

Chúng ta tiến hành bằng cách chọn chu trình chứa n, loại bỏ nó và giải đệ quy tập hợp còn lại. 

1. Cố định n là giá trị lớn nhất của chu kỳ cuối cùng. Chúng ta chọn tập con S chứa n. Tập hợp con này sẽ tạo thành một chu trình. 
2. Các phần tử còn lại tạo thành một bài toán nhỏ độc lập hơn. Khi S được chọn, tất cả các phần tử bên ngoài S phải xuất hiện trước S trong hoán vị cuối cùng. 
3. Với S được chọn, chúng ta đếm có bao nhiêu chu trình hợp lệ có thể được hình thành trong đó n là phần tử đầu tiên trong biểu diễn chu trình. Số chu kỳ như vậy là (|S| − 1)! vì sau khi cố định n ở phía trước, các phần tử còn lại có thể được sắp xếp tùy ý theo thứ tự chu trình. 
4. Chúng ta lặp lại các tập con S có thể có theo thứ tự đóng góp từ điển tăng dần. Đối với mỗi tập hợp con ứng cử viên, chúng tôi tính toán có bao nhiêu hoán vị đến từ nó bằng cách sử dụng DP,
