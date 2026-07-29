---
title: "CF 102767G - Singhal và phép nhân"
description: "Bài toán hỏi liệu một chuỗi số nguyên có chứa một đoạn liền kề mà tích của nó để lại số dư 1 khi chia cho độ dài của toàn bộ chuỗi hay không. Trong số tất cả các phân đoạn như vậy, chúng ta cần phân đoạn ngắn nhất. Nếu không có phân đoạn nào thỏa mãn điều kiện này thì câu trả lời là 0."
date: "2026-07-28T23:33:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102767
codeforces_index: "G"
codeforces_contest_name: "Codedigger Training Contest -Number Theory"
rating: 0
weight: 102767
solve_time_s: 59
verified: false
draft: false
---

[CF 102767G - Singhal và phép nhân](https://codeforces.com/problemset/problem/102767/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu một dãy số nguyên có chứa đoạn liền kề mà tích của nó để lại phần dư hay không.`1`khi chia cho độ dài của toàn bộ chuỗi. Trong số tất cả các phân đoạn như vậy, chúng ta cần phân đoạn ngắn nhất. Nếu không có phân đoạn nào thỏa mãn điều kiện này thì câu trả lời là`0`. Bài toán ban đầu định nghĩa điều kiện này là một mảng con có kết quả nhân phù hợp với`1`modulo`n`, Ở đâu`n`cũng là số phần tử của mảng. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được kích thước của chuỗi và sau đó là các giá trị của chuỗi. Các giá trị có thể rất lớn nhưng chỉ có phần dư của chúng theo modulo`n`ảnh hưởng đến câu trả lời. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị độ dài tối thiểu của một đoạn liền kề hợp lệ. 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là`100000`. Điều này ngay lập tức loại trừ việc kiểm tra mọi mảng con và nhân các phần tử của nó. Việc liệt kê trực tiếp sẽ yêu cầu xem xét một cách đại khái`n^2`phân khúc và duy trì từng sản phẩm riêng biệt sẽ thúc đẩy công việc hướng tới`O(n^3)`trong việc thực hiện tồi tệ nhất. Ngay cả phiên bản được tối ưu hóa mở rộng mọi điểm xuất phát và duy trì sản phẩm đang hoạt động vẫn cần`O(n^2)`hoạt động, quá lớn khi`n`đạt tới`100000`. Chúng ta cần sử dụng thuộc tính cho phép chúng ta xử lý chuỗi theo thời gian tuyến tính. 

Khó khăn chính xuất phát từ thực tế là phép nhân modulo thường không cho phép chia. Nếu chúng ta biết một tích tiền tố và muốn tích của phân khúc ở giữa, thì chúng ta không thể đơn giản chia một tích tiền tố này cho một tích số khác vì nghịch đảo mô đun chỉ tồn tại đối với các số nguyên tố cùng nhau với mô đun. 

Trường hợp cạnh đầu tiên xuất hiện khi chính một phần tử đã đưa ra câu trả lời. Ví dụ:```
Input:
1
5
7 1 3 4 8

Output:
1
```Phần tử đơn`1`có sản phẩm`1 mod 5`, do đó đoạn ngắn nhất có thể có độ dài bằng một. Một giải pháp chỉ tìm kiếm các sản phẩm tiền tố lặp lại và quên kiểm tra các phần tử riêng lẻ có thể bỏ lỡ điều này ngay lập tức. 

Một trường hợp quan trọng khác là khi một số giá trị không khả nghịch theo modulo`n`. Coi như:```
Input:
1
4
2 2 3 3

Output:
2
```Phân đoạn`[3, 3]`hoạt động vì`3 * 3 = 9`Và`9 mod 4 = 1`. Tuy nhiên, các phần tử`2`không thể xuất hiện trong bất kỳ phân đoạn hợp lệ nào vì mọi sản phẩm đều chứa yếu tố`2`chẵn và không bao giờ có thể để lại số dư`1`modulo`4`. Việc triển khai bất cẩn cố gắng áp dụng logic nghịch đảo mô-đun cho mọi phần tử sẽ thất bại vì`2`không có modulo nghịch đảo`4`. 

Trường hợp cạnh cuối cùng là khi toàn bộ mảng không chứa câu trả lời khả dĩ nào:```
Input:
1
2
2 2

Output:
0
```Mọi tích có thể đều là số chẵn, còn số dư mong muốn là`1`. Một giải pháp chỉ tìm kiếm các trạng thái lặp lại mà không tôn trọng điều kiện không thể đảo ngược trước tiên có thể cho rằng tồn tại một kết quả khớp không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu từ mọi điểm cuối bên trái có thể, nhân từng phần tử một trong khi mở rộng điểm cuối bên phải và kiểm tra xem sản phẩm hiện tại có trở thành`1`modulo`n`. Điều này đúng vì mọi mảng con có thể được xem xét chính xác một lần. Vấn đề là số lượng phân khúc. Một chuỗi độ dài`100000`chứa khoảng năm tỷ mảng con có thể có, do đó ngay cả một`O(1)`cập nhật cho mỗi tiện ích mở rộng sẽ quá chậm. 

Quan sát hữu ích xuất phát từ ý nghĩa của một sản phẩm bằng`1`modulo`n`. Nếu một số xuất hiện bên trong một phân đoạn hợp lệ thì số đó phải có mô đun nghịch đảo nhân`n`. Nói cách khác, nó phải nguyên tố cùng nhau với`n`. Nếu như`gcd(a_i, n) != 1`, thì mọi sản phẩm chứa`a_i`chia sẻ một yếu tố không tầm thường với`n`, nên nó không thể đồng dạng với`1`. 

Điều này có nghĩa là mảng có thể được tách thành các khối tối đa chỉ chứa các phần tử nguyên tố cùng nhau với`n`. Bất kỳ phân đoạn hợp lệ nào cũng phải nằm hoàn toàn bên trong một trong các khối này. 

Bên trong khối như vậy, mọi giá trị đều thuộc nhóm đơn vị nhân theo modulo`n`. Bây giờ việc phân chia theo mô-đun trở nên khả thi. Đặt một sản phẩm tiền tố là:$$p_i = a_1 a_2 \dots a_i \pmod n$$Đối với một đoạn từ`i + 1`ĐẾN`j`, sản phẩm của nó là:$$p_j \cdot p_i^{-1} \pmod n$$Điều này bằng`1`chính xác khi nào:$$p_j = p_i$$Vì vậy, vấn đề trở thành tìm khoảng cách ngắn nhất giữa hai sản phẩm tiền tố bằng nhau bên trong mỗi khối hợp lệ. Chúng ta có thể giải quyết vấn đề này bằng bản đồ băm lưu trữ vị trí mới nhất của mọi sản phẩm tiền tố. Khi một tích tiền tố xuất hiện trở lại, khoảng cách giữa hai vị trí chính là đáp án ứng viên. 

Brute-force hoạt động vì nó kiểm tra trực tiếp mọi sản phẩm có thể, nhưng nó bỏ qua cấu trúc đại số. Việc quan sát thấy các phân đoạn hợp lệ chỉ chứa các phần dư khả nghịch sẽ biến vấn đề thành việc phát hiện các trạng thái lặp lại trong một nhóm hữu hạn, việc này có thể được thực hiện chỉ bằng một lần quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Tôi sẽ tiếp tục với **Hướng dẫn thuật toán**, **Giải pháp Python**, **Các ví dụ đã thực hiện**, **Phân tích độ phức tạp**, **Các trường hợp thử nghiệm** và **Các trường hợp cạnh** trong phần tiếp theo.
