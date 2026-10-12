---
title: "CF 105158A - Một Lần Trong Đời"
description: "Cho một số nguyên dương n và một chữ số d. Chúng ta được phép chọn một số nguyên dương k khác và xét tích x = n · k. Mục đích là làm cho số kết quả này thỏa mãn một ràng buộc mẫu chữ số rất cụ thể."
date: "2026-06-27T11:04:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105158
codeforces_index: "A"
codeforces_contest_name: "2024 National Invitational of CCPC (Zhengzhou), 2024 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105158
solve_time_s: 45
verified: true
draft: false
---

[CF 105158A - Một lần trong đời](https://codeforces.com/problemset/problem/105158/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương`n`và một chữ số`d`. Chúng ta được phép chọn một số nguyên dương khác`k`, và chúng ta nhìn vào sản phẩm`x = n · k`. Mục đích là làm cho số kết quả này thỏa mãn một ràng buộc mẫu chữ số rất cụ thể. 

Một số được coi là hợp lệ nếu mỗi chữ số từ 1 đến 9 xuất hiện ít nhất một lần trong biểu diễn thập phân của nó và thêm vào đó là chữ số`d`xuất hiện ít nhất hai lần. Các số 0 không liên quan đến điều kiện và có thể xuất hiện tự do vì yêu cầu chỉ liên quan đến các chữ số từ 1 đến 9. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xây dựng bất kỳ số nguyên dương nào`k`nhân như vậy`n`qua`k`tạo ra một số hợp lệ. Ngoài ra còn có một sự đảm bảo rõ ràng rằng chúng ta không cần những câu trả lời cực kỳ lớn, vì`k`luôn có thể được chọn không vượt quá`2 · 10^10`. 

Khó khăn chính là hạn chế nằm ở thành phần chữ số của tích, không phải về tính chất số học của`n`hoặc`k`riêng lẻ. Điều này ngay lập tức loại trừ bất kỳ lực lượng vũ phu trực tiếp nào đối với`k`hoặc mô phỏng phép nhân cho tất cả các ứng viên, vì có tới`3 · 10^5`trường hợp thử nghiệm và`k`trải dài trên một miền rộng lớn. 

Một nỗ lực ngây thơ có thể thử tăng`k`từ 1 trở lên và kiểm tra xem`n·k`thỏa mãn điều kiện chữ số. Ngay cả khi chúng ta giả sử việc kiểm tra các chữ số là nhanh chóng thì phương pháp này có thể yêu cầu khám phá các giá trị cực lớn của`k`trong các trường hợp đối nghịch và sẽ không bao giờ kết thúc trong giới hạn nhất định. 

Một vấn đề tế nhị khác là phép nhân có thể thay đổi số lượng chữ số theo những cách không trực quan do mang. Ví dụ: cố gắng “nối các chữ số” bằng cách chọn cấu trúc`k`không hành xử tuyến tính, vì vậy chúng ta không thể dựa vào sự độc lập về mặt chữ số giữa`n`Và`k`. 

Thách thức thực sự là xây dựng một hệ số nhân buộc sản phẩm ở dạng thập phân được kiểm soát thay vì tìm kiếm. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng việc sửa chữa một ứng viên`k`, tính toán`x = n·k`, và kiểm tra xem`x`chứa các chữ số từ 1 đến 9 ít nhất một lần và chứa chữ số`d`ít nhất hai lần. Bản thân việc kiểm tra là tuyến tính theo số chữ số của`x`, nhiều nhất là khoảng 11 đối với ràng buộc lớn nhất trên`n`Và`k`. Vì vậy việc xác minh là rẻ. 

Vấn đề là không gian tìm kiếm. Ngay cả khi chúng ta hạn chế`k`ĐẾN`2 · 10^10`, trường hợp xấu nhất vẫn để lại hàng chục tỷ thí sinh. Không có cấu trúc nào gợi ý sự đơn điệu hoặc cắt tỉa, vì việc tăng`k`không bảo tồn hoặc phá hủy các mẫu chữ số theo cách có thể dự đoán được. 

Quan sát quan trọng là chúng ta không bắt buộc phải tối ưu hóa hoặc giảm thiểu bất cứ điều gì về`k`. Chúng ta chỉ cần sự tồn tại. Điều này cho phép chúng ta xây dựng`k`gián tiếp bằng cách buộc`n·k`để trở thành một con số được thiết kế cẩn thận. 

Thay vì suy nghĩ về việc lựa chọn`k`, chúng ta có thể suy nghĩ về việc lựa chọn sản phẩm mục tiêu`x`. Nếu chúng ta có thể xây dựng một số hợp lệ`x`đó là chia hết cho`n`, sau đó thiết lập`k = x / n`giải quyết vấn đề ngay lập tức. 

Điều này biến nhiệm vụ thành một bài toán chia hết mang tính xây dựng cổ điển: xây dựng một số có các chữ số bắt buộc cũng là bội số của`n`. 

Bí quyết chính là khai thác từng chữ số số học theo mô-đun. Chúng ta có thể xây dựng một số`x`bằng cách nối thêm các chữ số từ một mẫu cố định cho đến khi chúng ta đạt được số chia hết cho`n`. Kể từ khi chia cho`n ≤ 10^8`và chúng tôi đang thêm các chữ số, chúng tôi chỉ cần theo dõi`x mod n`, nó nhỏ. Khi chúng tôi đạt phần còn lại 0, chúng tôi có được một công trình hợp lệ. 

Để đảm bảo ràng buộc về chữ số, chúng tôi không tìm kiếm ngẫu nhiên. Thay vào đó, chúng tôi xây dựng một mẫu cơ sở đã chứa tất cả các chữ số từ 1 đến 9 và đảm bảo chữ số`d`xuất hiện ít nhất hai lần. Một chuỗi cố định đơn giản như`"123456789"`cộng thêm một lần xuất hiện của`d`đã thỏa mãn điều kiện chữ số về mặt cấu trúc. 

Sau đó, chúng tôi nối các chữ số vào cơ sở này cho đến khi điều kiện mô-đun được thỏa mãn. 

Điều này làm giảm vấn đề khám phá một không gian trạng thái có kích thước`9 * n`(thời gian còn lại của vị trí thi công), có thể quản lý được vì`n`tùy thuộc vào`10^8`nhưng quá trình chuyển đổi mang tính quyết định và chúng ta chỉ cần một con đường thành công. Trong thực tế, giải pháp dự định sẽ tránh BFS đầy đủ trên tất cả các trạng thái và thay vào đó sử dụng cấu trúc mô-đun tham lam để đảm bảo tấn công nhanh chóng bằng cách mở rộng với các mẫu lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn k | O(chữ số lớn ×) | O(1) | Quá chậm | 
| Xây dựng x với tìm kiếm mô-đun | O(n · 10) khấu hao cho mỗi lần kiểm tra (khái niệm) | O(n) trường hợp xấu nhất, hằng số được tối ưu hóa trong thực tế | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách làm việc trực tiếp với sản phẩm mục tiêu`x = n·k`. 

1. Bắt đầu bằng cách xây dựng chuỗi cơ sở`S`đã thỏa mãn yêu cầu về chữ số mà bỏ qua tính chia hết. Sự lựa chọn tự nhiên là`"123456789"`và sau đó chúng tôi chèn thêm một lần xuất hiện của chữ số`d`ở đâu đó, ví dụ như ở cuối. Điều này đảm bảo tất cả các chữ số 1-9 xuất hiện và`d`xuất hiện ít nhất hai lần. 
2. Giải thích chuỗi này dưới dạng số modulo`n`. Chúng tôi tính toán phần còn lại của nó một cách hiệu quả mà không cần lưu trữ các số nguyên lớn, chỉ theo dõi`(remainder * 10 + digit) % n`khi chúng tôi quét các chữ số của`S`. 
3. Nếu phần còn lại bằng 0 thì chúng ta đã hoàn thành. Số được biểu thị bằng`S`chia hết cho`n`, Vì thế`k = S / n`hoạt động. 
4. Mặt khác, chúng ta mở rộng số bằng cách thêm các chữ số. Mỗi chữ số được nối thêm sẽ cập nhật phần còn lại một cách xác định. Chúng tôi thử các chữ số theo một thứ tự cố định, thông thường`0`ĐẾN`9`và với mỗi phần mở rộng, chúng tôi tính toán phần còn lại mới. 
5. Chúng ta tiếp tục quá trình này cho đến khi đạt được số dư bằng 0. Vì chúng tôi đang khám phá một cách hiệu quả các dư lượng có thể tiếp cận được trong quá trình chuyển đổi chữ số, nên cuối cùng chúng tôi được đảm bảo sẽ đóng một chu trình đạt đến 0 do tính hữu hạn của các trạng thái. 
6. Khi chúng tôi tìm thấy một số hợp lệ`x`, chúng tôi tính toán`k = x // n`và xuất nó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, công trình duy trì một số cụ thể có phần dư theo modulo`n`được biết chính xác. Mỗi phần mở rộng tương ứng với việc nhân với 10 và thêm một chữ số, điều này duy trì tính chính xác của bản cập nhật còn lại. Không gian trạng thái có thể`(remainder, constructed prefix conditions)`là hữu hạn, do đó, bất kỳ quá trình nào tiếp tục mở rộng một cách xác định đều không thể tránh được việc lặp lại trạng thái hoặc đạt đến phần dư làm đóng điều kiện chia hết. Vì chúng tôi không bao giờ loại bỏ các cấu hình chữ số hợp lệ và chỉ nối thêm các chữ số, nên số được xây dựng cuối cùng vẫn chứa tất cả các chữ số từ 1 đến 9 và bảo toàn bội số cần thiết của`d`. 

## Giải pháp Python```python
import sys
input = sys.std
```
