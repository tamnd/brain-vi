---
title: "CF 2178B - Impost hoặc Sus"
description: "Chúng ta bắt đầu bằng một chuỗi chỉ gồm s và u. Chúng ta có thể liên tục chọn bất kỳ vị trí nào chứa u và thay đổi nó thành s. Chúng ta không được phép đổi chữ s trở lại chữ u. Chuỗi cuối cùng phải thỏa mãn hai điều kiện. Đầu tiên, nó phải chứa ít nhất hai lần xuất hiện của s."
date: "2026-06-07T22:22:54+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "B"
codeforces_contest_name: "Good Bye 2025"
rating: 900
weight: 2178
solve_time_s: 155
verified: false
draft: false
---

[CF 2178B - Impost hoặc Sus](https://codeforces.com/problemset/problem/2178/B) 

**Xếp hạng:** 900 
**Tags:** dp, tham lam, triển khai, chuỗi 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi chỉ bao gồm`s`Và`u`. 

Chúng tôi có thể liên tục chọn bất kỳ vị trí nào có chứa`u`và thay đổi nó thành`s`. Chúng tôi không được phép thay đổi một`s`quay lại`u`. 

Chuỗi cuối cùng phải thỏa mãn hai điều kiện. Đầu tiên, nó phải chứa ít nhất hai lần xuất hiện của`s`. Thứ hai, mọi thứ còn lại`u`phải ngồi chính xác ở giữa hai điểm gần nhất của nó`s`nhân vật. 

Nhiệm vụ là tìm số lượng tối thiểu`u → s`các thao tác cần thiết. 

Tổng độ dài của tất cả các chuỗi trong các trường hợp thử nghiệm tối đa là`2 · 10^5`. Bất kỳ giải pháp nào chỉ kiểm tra từng ký tự với số lần không đổi đều đủ nhanh. Phương pháp bậc hai bị loại trừ vì một bài kiểm tra đơn lẻ có thể chứa`200000`nhân vật. 

Phần khó khăn là hiểu chuỗi cuối cùng hợp lệ thực sự trông như thế nào. 

Xét hai liên tiếp`s`các ký tự ở chuỗi cuối cùng. 

Nếu khoảng cách của họ là`1`, chúng tạo thành`"ss"`và không có`u`giữa họ. 

Nếu khoảng cách của họ là`2`, chúng tạo thành`"sus"`, và vị trí ở giữa được căn giữa một cách hoàn hảo. 

Nếu khoảng cách lớn hơn`2`, có nhiều vị trí giữa chúng. Chỉ có điểm giữa chính xác sẽ cách xa cả hai`s`, vì vậy mọi vị trí khác sẽ vi phạm điều kiện. 

Điều này có nghĩa là trong bất kỳ chuỗi đáng ngờ nào: 

- mỗi`u`được bao quanh bởi`s`ở cả hai bên, 
- không có hai`u`các ký tự liền kề nhau, 
- ký tự đầu tiên và cuối cùng không được`u`. 

Ví dụ,`"suss"`là hợp lệ bởi vì duy nhất`u`nằm giữa hai`s`nhân vật. 

Ví dụ,`"suus"`không hợp lệ vì hai ở giữa`u`cả hai ký tự không thể được căn giữa giữa cùng một cặp ký tự`s`. 

Một sai lầm phổ biến là tập trung vào khoảng cách giữa hiện tại`s`nhân vật. Vì chúng tôi được phép tạo mới`s`ký tự ở bất cứ đâu bằng cách chuyển đổi`u`, câu hỏi quan trọng không phải là dòng điện ở đâu`s`nhân vật là gì, nhưng đó là`u`các ký tự chúng tôi chọn để giữ. 

Coi như:```
uuuu
```Một ý tưởng tham lam có thể cố gắng đặt`s`chỉ ở phần cuối. Điều đó mang lại`"suus"`, vẫn không hợp lệ. Câu trả lời tối ưu là`3`, sản xuất một cái gì đó như`"suss"`. 

Một trường hợp dễ bỏ sót khác là:```
sus
```Câu trả lời là`0`. Chuỗi đã đáp ứng tất cả các yêu cầu. 

Cuối cùng:```
uuu
```Chỉ có phần giữa`u`có thể ở lại. Cả hai vị trí cuối cùng phải trở thành`s`, vậy câu trả lời là`2`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem xét mọi tập hợp con của`u`vị trí để chuyển đổi thành`s`. Với mỗi lựa chọn, chúng ta có thể kiểm tra xem chuỗi kết quả có đáng ngờ hay không. 

Nếu một chuỗi chứa`k`sự xuất hiện của`u`, điều này đòi hỏi phải kiểm tra`2^k`khả năng. Với`k`có khả năng ở gần`200000`, điều này hoàn toàn không thể thực hiện được. 

Để làm tốt hơn, chúng ta cần mô tả đặc điểm nào`u`các nhân vật được phép sống sót. 

Từ quan sát cấu trúc ở trên, mọi người sống sót`u`phải thỏa mãn hai tính chất. 

Đầu tiên, nó không thể ở một trong hai đầu của chuỗi. 

Thứ hai, nó không thể liền kề với một cái khác còn sống sót`u`. 

Không có gì khác quan trọng. 

Nếu một`u`là nội bộ và tất cả các vị trí lân cận trở thành`s`, sau đó nó tự động nằm giữa hai`s`nhân vật ở xa`1`ở cả hai bên. 

Điều này biến bài toán thành bài toán giữ giá trị lớn nhất. 

Chúng tôi muốn giữ càng nhiều bản gốc`u`ký tự càng tốt, tùy thuộc vào: 

- giữ`u`vị trí không ở cuối, 
- không có hai giữ`u`các vị trí liền kề nhau. 

Mọi`u`không được giữ lại phải được chuyển đổi thành`s`. 

Vì thế:```
answer = total_u - maximum_keepable_u
```Tìm số lượng tối đa các vị trí được giữ không liền kề là đường dẫn chuẩn DP. 

Cho phép:

-`dp0`= số lượng tốt nhất được giữ lại`u`cho đến nay khi vị trí trước đó không còn được giữ nguyên`u`, 
-`dp1`= số lượng tốt nhất được giữ lại`u`cho đến nay khi vị trí trước đó được giữ nguyên`u`. 

Chúng tôi quét từ trái sang phải và cập nhật các trạng thái này theo thời gian không đổi cho mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k · n) | O(n) | Quá chậm | 
| DP tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng`u`các ký tự trong chuỗi. 
2. Duy trì hai trạng thái DP.`dp0`lưu trữ số lượng tối đa được giữ`u`ký tự cho đến nay khi vị trí trước đó không được giữ.`dp1`lưu trữ số lượng tối đa được giữ`u`ký tự cho đến nay khi vị trí trước đó được giữ nguyên. 
3. Xử lý chuỗi từ trái sang phải. 
4. Nếu ký tự hiện tại là`s`, nó không thể được giữ như một`u`. 

Hợp nhất cả hai trạng thái trước đó vào trạng thái mới`dp0`. 
5. Nếu ký tự hiện tại là`u`, chúng ta có hai lựa chọn. 

Chuyển đổi nó thành`s`. 

Trong trường hợp đó, trạng thái trước đó không thành vấn đề và chúng ta chuyển sang`dp0`. 
6. Nếu ký tự hiện tại là một vị trí bên trong, nghĩa là nó không phải là ký tự đầu tiên cũng không phải là ký tự cuối cùng, chúng ta có thể giữ nguyên nó như`u`. 

Để giữ nó, vị trí trước đó không được giữ lại`u`, do đó quá trình chuyển đổi chỉ đến từ`dp0`. 
7. Sau khi xử lý tất cả các vị trí, số lượng tối đa có thể lưu giữ`u`nhân vật là`max(dp0, dp1)`. 
8. Câu trả lời là:```
total_u - maximum_keepable_u
```### Tại sao nó hoạt động 

Một chuỗi đáng ngờ hoàn toàn được xác định bởi chuỗi gốc nào`u`ký tự không thay đổi. 

Một phần còn lại`u`mu
