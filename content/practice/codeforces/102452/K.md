---
title: "CF 102452K - Dự án trọng điểm"
description: "Chúng tôi có hai nhóm kỹ sư, kỹ sư thuật toán và kỹ sư phần mềm. Mỗi kỹ sư làm việc tại một trong n tòa nhà và có chi phí phân công công việc riêng."
date: "2026-08-12T08:33:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 284
verified: true
draft: false
---

[CF 102452K - Dự án trọng điểm](https://codeforces.com/problemset/problem/102452/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai nhóm kỹ sư, kỹ sư thuật toán và kỹ sư phần mềm. Mỗi kỹ sư làm việc tại một trong n tòa nhà và có chi phí phân công công việc riêng. Đối với mỗi cặp được chọn, một kỹ sư từ mỗi nhóm sẽ được chọn và cặp này sẽ trả thêm khoảng cách giữa các tòa nhà của họ. 

Với mỗi k từ 1 đến m, chúng ta cần tổng chi phí tối thiểu để chọn chính xác k cặp thuật toán/phần mềm rời nhau. 

Các tòa nhà tạo thành một con đường có trọng số. Nếu p i ​ là tọa độ của tòa nhà i, thu được bằng tổng tiền tố của khoảng cách, thì chi phí vận chuyển giữa tòa nhà i và j là ∣p i ​ −p j ​ ∣. 

Các giới hạn được cố tình không đối xứng. Có thể chỉ có 800 tòa nhà nhưng có tới 50000 kỹ sư. Điều này loại trừ mọi thứ bậc hai theo m, kể cả việc xây dựng tất cả m 2 cặp có thể. Nó cũng làm cho việc triển khai luồng chi phí tối thiểu thông thường trở nên quá tốn kém nếu mỗi kỹ sư được biểu diễn dưới dạng một cạnh riêng biệt và đường tăng cường ngắn nhất được tính toán lại cho tất cả m đơn vị. Thứ nguyên hữu ích là n, không phải m, vì vậy thuật toán cuối cùng sẽ nhắm tới khoảng O(nm). 

Có một số trường hợp dễ gây ra việc thực hiện sai. 

Đầu tiên, cả hai kỹ sư đều có thể ở cùng một tòa nhà. Ví dụ,```

```có câu trả lời```

```vì không tốn chi phí vận chuyển. Việc triển khai luôn thêm khoảng cách của một cạnh liền kề sẽ tính phí không chính xác 5. 

Thứ hai, đường tăng cường có thể có chi phí vận chuyển âm trong mạng dư. Coi như```

```Cặp đầu tiên được hình thành với giá rẻ nhất giữa tòa nhà 1 và tòa nhà 2, với chi phí 12. Sau đó, luồng dư đi từ tòa nhà 1 đến tòa nhà 2. Đường tăng cường thứ hai đi theo hướng ngược lại và hủy bỏ luồng vận chuyển hiện có đó, do đó chi phí biên của nó là −8. Các câu trả lời cuối cùng là```

```Một thuật toán tham lam chỉ xem xét chi phí vận chuyển dương sẽ bỏ lỡ câu trả lời thứ hai. 

Thứ ba, một số kỹ sư có thể làm việc trong cùng một tòa nhà. Ví dụ,```

```có câu trả lời```

```bởi vì chi phí vận chuyển luôn bằng 0 và chúng tôi chỉ cần chọn hai kỹ sư rẻ nhất từ ​​mỗi nhóm. Đối xử với một tòa nhà như thể nó chỉ có một kỹ sư sẽ thất bại ở đây. 

Cuối cùng, chi phí có thể vượt quá số nguyên 32 bit. Với 50000 cặp, riêng chi phí chuyển nhượng có thể lên tới 5⋅10 12 và việc vận chuyển có thể tăng thêm nhiều đơn đặt hàng lớn hơn. Số nguyên Python tự động xử lý việc này, trong khi việc triển khai C++ sẽ cần số nguyên 64 bit. 

## Phương pháp tiếp cận 

Công thức trực tiếp nhất là mạng lưới dòng chi phí tối thiểu. Tạo một nguồn và một phần chìm, một đỉnh cho mỗi tòa nhà, một cạnh từ nguồn đến tòa nhà của mọi kỹ sư thuật toán với chi phí phân công của kỹ sư đó và một cạnh từ tòa nhà của mọi kỹ sư phần mềm đến phần chìm với chi phí của kỹ sư đó. Các công trình liền kề được kết nối bằng các cạnh giao thông của chi phí d i ​. Gửi một đơn vị luồng tương ứng với việc chọn một kỹ sư thuật toán, di chuyển qua các tòa nhà và chọn một kỹ sư phần mềm. 

Công thức này đúng vì mỗi đơn vị luồng chọn chính xác một kỹ sư từ mỗi nhóm và đường đi qua biểu đồ tòa nhà trả chính xác khoảng cách vận chuyển cần thiết. Gửi k đơn vị sẽ mang lại kết quả tối ưu cho k cặp. 

Vấn đề là hiệu suất. Việc triển khai đường dẫn ngắn nhất liên tiếp theo tiêu chuẩn sẽ có các cạnh O(m+n) và thực hiện tối đa m phép tính đường dẫn ngắn nhất. Ngay cả khi sử dụng Dijkstra có tiềm năng, trường hợp xấu nhất là đại khái 

O(m(m+n)logn), 

có nghĩa là khoảng năm tỷ độ giãn cạnh cho m=50000. Bài xã luận chính thức cũng chỉ ra rằng việc triển khai dòng chi phí tối thiểu tiêu chuẩn là quá chậm đối với những hạn chế này. 

Quan sát quan trọng là số lượng lớn các cạnh kỹ sư chủ yếu là nhân tạo. Tại một tòa nhà, tất cả các cạnh nguồn đều có điểm cuối giống hệt nhau, chỉ khác nhau về chi phí phân công. Nếu chúng ta chọn q kỹ sư thuật toán từ tòa nhà đó thì lựa chọn tối ưu luôn là q kỹ sư rẻ nhất. Vì vậy, chúng tôi có thể sắp xếp chi phí ở mỗi tòa nhà và chỉ hiển thị chi phí chưa sử dụng tiếp theo. 

Mạng còn lại chỉ có n đỉnh xây dựng và một đường dẫn giữa chúng. Quan trọng hơn, đường tăng cường ngắn nhất có dạng rất đơn giản. Nó bắt đầu bằng việc chọn một kỹ sư thuật toán chưa được sử dụng ở tòa nhà i nào đó, đi theo con đường duy nhất giữa tòa nhà i và j, và kết thúc bằng việc chọn một kỹ sư phần mềm chưa được sử dụng ở tòa nhà j. 

Chúng tôi có thể duy trì dòng chảy hiện tại trên mọi cạnh của tòa nhà một cách ngầm định. Nếu một đường tăng cường đi từ i đến j, nó sẽ thay đổi mọi cạnh giữa chúng thêm +1 khi i<j, hoặc −1 khi i>j. Một bản cập nhật phạm vi có thể được biểu thị bằng một mảng khác biệt, do đó việc thay đổi tất cả các cạnh đó sẽ tốn O(1). 

Điều đó chỉ để lại tính toán đường đi ngắn nhất. Vì biểu đồ tòa nhà là một đường thẳng nên chúng ta có thể quét các tòa nhà một lần và tính toán đường đi tốt nhất theo cả hai hướng. Chi phí còn lại khi đi qua một cạnh chỉ phụ thuộc vào dấu của dòng chảy hiện tại của nó. Nếu dòng điện đi từ trái sang phải, việc di chuyển từ phải sang trái sẽ hủy bỏ một đơn vị và có giá −d i ​. Nếu dòng chảy theo hướng ngược lại, việc di chuyển từ phải sang trái sẽ thêm một đơn vị khác và tốn +d i ​. Khi luồng bằng 0, cả hai hướng đều có giá +d i ​. 

Do đó, mọi phép tăng thêm đều mất O(n) và tất cả m câu trả lời đều thu được trong O(nm). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Dòng chi phí tối thiểu rõ ràng | O(m(m+n)logn) | O(m+n) | Quá chậm | 
| Mô phỏng dư nén | O(nm+mlogm) | O(n+m) | Đã chấp nhận | 

Thuật ngữ mlogm xuất phát từ việc phân loại chi phí kỹ sư bên trong các tòa nhà. Thuật ngữ chiếm ưu thế là nm. 

## Hướng dẫn thuật toán

1. Chuyển đổi khoảng cách tòa nhà thành tọa độ. Đặt p 1 ​ =0, và p i+1 ​ =p i ​ +d i ​. Chúng tôi thực sự không cần những tọa độ này trong quá trình triển khai vì chi phí vận chuyển còn lại có thể được tích lũy trực tiếp trong khi quét đường đi. 
2. Nhóm chi phí phân công của tất cả các kỹ sư thuật toán theo cách xây dựng và thực hiện tương tự đối với các kỹ sư phần mềm. Sắp xếp mọi nhóm ngày càng tăng. Tại bất kỳ thời điểm nào, chi phí đầu tiên chưa được sử dụng tại một tòa nhà là cạnh nguồn hoặc cạnh đích duy nhất có thể tham gia vào đường dẫn tăng cường tiếp theo. 
3. Duy trì dòng điện fi ​ ở rìa giữa tòa nhà i và i+1. Thay vì lưu trữ rõ ràng mọi f i ​ sau mỗi lần cập nhật phạm vi, hãy giữ một mảng khác biệt. Nếu một đường tăng cường thay đổi mọi cạnh từ l đến r−1 bằng +1, hãy cộng 1 vào hiệu tại l và trừ 1 tại r. Điều tương tự cũng xảy ra với bản cập nhật −1. 
4. Trong một lần quét từ trái sang phải, hãy tái tạo lại dòng điện trên mỗi cạnh. Đối với một cạnh có khoảng cách d, hãy tính chi phí còn lại theo cả hai hướng. Nếu f i ​ >0, di chuyển từ trái sang phải tốn d và di chuyển từ phải sang trái tốn −d. Nếu f i ​ <0, dấu hiệu đảo ngược. Nếu f i ​ =0, cả hai hướng đều có giá d. 
5. Trong khi quét, hãy duy trì đường dẫn từ nguồn đến tòa nhà hiện tại rẻ nhất tiếp cận từ bên trái. Giả sử tòa nhà ban đầu của nó là s và chi phí vận chuyển còn lại từ trái sang phải tích lũy là P. Đóng góp của nó trước khi chọn kỹ sư phần mềm tại tòa nhà j là 

A s ​ −P s ​ +B j ​ +P j ​ . 

Việc giữ giá trị tối thiểu của A s ​ −P s ​ cho phép chúng ta đánh giá tất cả s<jj trong thời gian không đổi tại mỗi tòa nhà. 
6. Đồng thời, duy trì điểm cuối phần mềm rẻ nhất được nhìn thấy ở bên trái. Nếu Q i ​ là chi phí còn lại tích lũy để đi qua các cạnh theo hướng từ phải sang trái, thì đường đi từ xây dựng thuật toán i đến xây dựng phần mềm j trước đó có chi phí 

A i ​ +Q i ​ +B j ​ −Q j ​ . 

Giữ mức B j ​ −Q j ​ tối thiểu cho phép chúng ta đánh giá mọi i>j trong cùng một lần quét. 
7. Sử dụng các đường tăng cường từ trái sang phải và từ phải sang trái tốt nhất với giá rẻ hơn. Chi phí của nó là chi phí biên của việc tăng giá trị luồng từ k−1 lên k. Thêm chi phí cận biên này vào tổng chi phí đang hoạt động và đưa ra kết quả là k. 
8. Đưa con trỏ tới thuật toán và tòa nhà phần mềm đã chọn. Vì các kỹ sư tại một tòa nhà có các điểm cuối giống hệt nhau trong mạng lưới dòng chảy nên kỹ sư được chọn tiếp theo phải là kỹ sư rẻ nhất tiếp theo. 
9. Cập nhật mảng khác biệt cho đường vận chuyển. Nếu tòa nhà thuật toán đã chọn nằm ở bên trái tòa nhà phần mềm, hãy thêm một đơn vị vào mỗi cạnh giữa chúng. Nếu nó ở bên phải, hãy trừ đi một đơn vị ở mỗi cạnh giữa chúng. Khi cả hai đều ở trong cùng một tòa nhà, lợi thế vận chuyển không thay đổi. 

Điều bất biến là sau k lần lặp, luồng được duy trì là luồng có chi phí tối thiểu có giá trị k. Lần lặp tiếp theo chọn đường dẫn từ nguồn đến đích có chi phí tối thiểu, chính xác như trong đường dẫn ngắn nhất liên tiếp. Điểm nén duy nhất là các cạnh kỹ sư song song được biểu thị bằng chi phí đã sắp xếp của chúng và mạng dư dòng được đánh giá bằng một lần quét thay vì thuật toán đường đi ngắn nhất chung. Đường đi ngắn nhất liên tiếp duy trì chi phí tối thiểu sau mỗi lần tăng, do đó giá trị tích lũy sau mỗi lần lặp chính xác là giá trị tối ưu cần thiết. 

## Giải pháp Python```
Python
```Giai đoạn nhóm và sắp xếp tạo ra`alg`Và`soft`. Các mảng`cur_a`Và`cur_b`giữ chính xác lợi thế kỹ sư còn lại có thể được sử dụng tiếp theo tại mỗi tòa nhà. Khi một người được chọn, con trỏ của nó sẽ tiến tới kỹ sư rẻ nhất tiếp theo. 

các`diff`mảng đại diện cho luồng vận chuyển đã ký. Nếu một con đường từ tòa nhà`i`để xây dựng`j`được tăng lên, mỗi cạnh trong khoảng giữa chúng thay đổi một. Hai bản cập nhật điểm cuối trong`diff`mã hóa toàn bộ phạm vi thay đổi mà không cần chạm vào các cạnh riêng lẻ của nó. 

Quá trình quét chính chỉ tái tạo lại luồng hiện tại của mỗi cạnh khi nó đến cạnh đó. Các dấu hiệu của`w_lr`Và`w_rl`là chi tiết dòng dư trung tâm. Luồng hiện tại có thể bị hủy, đó là lý do tại sao một trong những hướng này có thể có chi phí âm. Việc bỏ qua điều này sẽ tạo ra câu trả lời sai trong trường hợp tuyến đường vận chuyển đắt tiền trước đó sau đó bị hủy bỏ. 

Mức tối thiểu đầu tiên, được lưu trữ trong`best_a`, xử lý các đường dẫn tăng cường có điểm cuối thuật toán ở bên trái điểm cuối phần mềm. Mức tối thiểu thứ hai,`best_b`, xử lý theo hướng ngược lại. Nó chỉ được cập nhật sau khi xem xét tòa nhà hiện tại, vì vậy trường hợp thứ hai luôn có j<i, trong khi trường hợp đầu tiên cho phép i=j. 

Câu trả lời được tích lũy dưới dạng chi phí cận biên. Chi phí cận biên không nhất thiết phải dương. Trong ví dụ tùy chỉnh thứ hai bên dưới, chi phí cận biên thứ hai là âm vì đường dư mới sẽ hủy việc vận chuyển mà đơn vị đầu tiên đã thanh toán. 

Số nguyên Python không bị giới hạn, do đó không cần xử lý 64-bit rõ ràng. 

## Ví dụ đã hoạt động 

Ví dụ đầu tiên là mẫu chính thức.```

```Các kỹ sư thuật toán đang ở tòa nhà 1,1,4, với chi phí 1,2,6. Các kỹ sư phần mềm ở tòa nhà 2,2,3, với chi phí 1,2,5. 

| k | Xây dựng thuật toán chọn lọc | Xây dựng phần mềm chọn lọc | Chi phí cận biên | Cập nhật luồng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | cạnh 1: +1 | 3 | 
| 2 | 1 | 2 | 5 | cạnh 1: +1 | 8 | 
| 3 | 4 | 3 | 12 | cạnh 3: −1 | 20 | 

Đối với lần tăng cường đầu tiên, cặp rẻ nhất là kỹ sư thuật toán ở tòa nhà 1 với kỹ sư phần mềm ở tòa nhà 2, chi phí 1+1+1=3. Lần tăng cường thứ hai sử dụng kỹ sư thứ hai tại mỗi tòa nhà đó, chi phí là 2+2+1=5. Đối với cặp cuối cùng, các kỹ sư còn lại ở tòa nhà 4 và 3, cho 6+5+1=12. Do đó, đầu ra yêu cầu là 3,8,20. 

Ví dụ thứ hai chứng minh tại sao chi phí vận chuyển còn lại có thể âm.```

```| k | Xây dựng thuật toán chọn lọc | Xây dựng phần mềm chọn lọc | Vận chuyển dư thừa | Chi phí cận biên | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | +10 | 12 | 12 | 
| 2 | 2 | 1 | −10 | -8 | 4 | 

Cặp đầu tiên gửi một đơn vị từ tòa nhà 1 đến tòa nhà 2, tạo luồng +1 ở cạnh duy nhất. Đối với cặp thứ hai, đường tăng cường đi từ tòa nhà 2 trở lại tòa nhà 1. Việc truyền tải đó sẽ hủy bỏ luồng hiện có, do đó chi phí vận chuyển còn lại của nó là −10. Việc cộng hai chi phí kỹ sư sẽ có chi phí biên là −8, giảm tổng số từ 12 xuống 4. Giải pháp cuối cùng bao gồm hai cặp tòa nhà giống nhau sau khi định tuyến lại phần còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mn+mlogm) | Mỗi lần mở rộng sẽ quét tất cả n tòa nhà một lần và tất cả chi phí kỹ sư được sắp xếp một lần | 
| Không gian | O(m+n) | Chi phí kỹ sư, con trỏ, chi phí hiện tại và mảng chênh lệch luồng đường dẫn được lưu trữ | 

Với n<800 và m<50000, vòng lặp chính thực hiện tối đa 40 triệu lần lặp xây dựng. Đó là tỷ lệ dự định: tham số lớn m xuất hiện tuyến tính, trong khi số lượng tòa nhà nhỏ xuất hiện dưới dạng yếu tố còn lại. Thuật toán tránh việc xây dựng các cặp kỹ sư có thể có O(m 2 ) và tránh tính toán đường đi ngắn nhất O(m) trên biểu đồ chứa O(m) các cạnh kỹ sư rõ ràng. 

## Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, cả kỹ sư tại tòa nhà 1 |`12`| Tối thiểu n,m, không vận chuyển, xử lý điểm cuối | 
| Hai tòa nhà có kỹ sư chéo nhau |`12`,`4`| Chi phí dư âm và hủy bỏ dòng chảy | 
| Tất cả kỹ sư tại tòa nhà 2 |`3`,`10`| Nhiều kỹ sư tại một tòa nhà và không cần vận chuyển | 
| n=800,m=50000, tất cả chi phí và vị trí bằng nhau | 2,4,…,100000 | Kích thước đầu vào tối đa, phụ thuộc tuyến tính vào m, đầu ra lớn | 

## Vỏ cạnh 

Khi cả hai kỹ sư đều ở trong cùng một tòa nhà, đường tăng cường không có lợi thế vận chuyển. TRONG```

```quá trình quét xem xét kỹ sư thuật toán và kỹ sư phần mềm ở cùng một vị trí, do đó, ứng viên chuyển tiếp sử dụng chi phí vận chuyển tích lũy bằng 0. Câu trả lời là 5+7=12, đúng như yêu cầu. 

Khi một đường tăng cường đi ngược lại với dòng chảy hiện tại, đóng góp vận chuyển của nó có thể âm. TRONG```

```lần tăng thêm đầu tiên sẽ gửi một đơn vị từ tòa nhà 1 đến tòa nhà 2, do đó luồng cạnh trở thành +1. Ở lần quét thứ hai, việc di chuyển từ tòa nhà 2 sang tòa nhà 1 có chi phí còn lại là −10. Chi phí của kỹ sư thứ hai cộng thêm 2, tạo ra chi phí biên là −8. Tổng số lượt chạy thay đổi từ 12 thành 4, tương ứng với việc định tuyến lại hai cặp để cuối cùng cả hai gặp nhau tại tòa nhà của chính họ. 

Khi nhiều kỹ sư chia sẻ một tòa nhà, danh sách được sắp xếp sẽ tạo ra hành vi chính xác. TRONG```

```chi phí vận chuyển luôn bằng không. Đầu tiên, thuật toán hiển thị chi phí 1 và 2, cho ra chi phí cận biên là 3. Sau đó, thuật toán nâng cao cả hai con trỏ xây dựng và hiển thị 4 và 3, cho ra chi phí cận biên là 7. Tổng cộng là 3 và 10. 

Tại tòa nhà ranh giới 1, không có cạnh nào trước nó, vì vậy ứng cử viên đường đi ngược lại không được xem xét cho đến khi có điểm cuối phần mềm hoàn toàn sớm hơn. Tại tòa nhà n, không có cạnh đường dẫn đi ra nên quá trình quét dừng lại sau khi đánh giá điểm cuối. Hai ranh giới này được xử lý trực tiếp bởi`i == n - 1`điều kiện và bằng cách chỉ cập nhật mức tối thiểu ngược lại sau khi đánh giá tòa nhà hiện tại. 

Cuối cùng, khi các tòa nhà được chọn ở gần nhau, bản cập nhật phạm vi sẽ thay đổi chính xác một cạnh luồng. Khi chúng bằng nhau thì cạnh luồng không thay đổi gì cả. Bản cập nhật mảng khác biệt sử dụng`[min(i,j), max(i,j))`, đây chính xác là tập hợp các cạnh đường dẫn giữa hai tòa nhà và tránh được lỗi sai lệch phổ biến khi sửa đổi một cạnh bên ngoài cặp.
