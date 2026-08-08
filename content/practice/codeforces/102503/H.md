---
title: "CF 102503H - Vấn đề về tờ giấy"
description: "Mỗi tờ có thể được xác định bằng số trang nhỏ hơn của nó. Trang i chứa các trang i và i+1, vì vậy hai trang a và b chỉ tạo thành một cặp thần thánh khi nhãn lớn hơn nhãn nhỏ ít nhất hai trang và trang lớn hơn xuất hiện sớm hơn trong ngăn xếp."
date: "2026-08-07T04:37:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "H"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 444
verified: true
draft: false
---

[CF 102503H - Sự cố về bảng tính](https://codeforces.com/problemset/problem/102503/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi tờ có thể được xác định bằng số trang nhỏ hơn của nó. Tờ giấy`i`chứa các trang`i`Và`i+1`, vậy là hai tờ`a`Và`b`chỉ tạo một cặp thần thánh khi nhãn lớn hơn ít nhất hai nhãn lớn hơn nhãn nhỏ hơn và trang tính lớn hơn xuất hiện sớm hơn trong ngăn xếp. 

Nhiệm vụ là đếm hoán vị của nhãn`1..n`số lượng các cặp như vậy chính xác là`k`. Hướng của các tờ giấy không quan trọng nên mỗi thứ tự của nhãn là một cách sắp xếp khác nhau. 

Những hạn chế quan trọng là cả hai`n`Và`k`nhiều nhất là`750`, trong khi có thể có nhiều trường hợp thử nghiệm. Việc liệt kê lực lượng vũ phu giai thừa là không thể bởi vì ngay cả`10!`sự sắp xếp đã có hàng triệu, và`750!`vượt xa mọi cách tiếp cận trực tiếp. Quan sát hữu ích là chúng ta chỉ cần 750 hệ số đầu tiên của đa thức đếm, vì vậy lời giải nên xây dựng câu trả lời tăng dần và loại bỏ thông tin về các giá trị lớn hơn của`k`. 

Một sai lầm phổ biến là coi đây là cách đếm đảo ngược thông thường. Ví dụ như các tờ`3`Và`2`không phải là một cặp thần thánh dù chúng được xếp theo thứ tự giảm dần vì các trang của chúng chồng lên nhau. Các nhãn liền kề là đặc biệt và không được đóng góp. 

Một trường hợp cạnh khác là`k = 0`. Đối với đầu vào```

```câu trả lời là`2`, bởi vì cả hai thứ tự có thể có của hai tờ đều không có cặp thần thánh. Một giải pháp đếm các nghịch đảo thông thường sẽ trả về không chính xác`1`. 

Trường hợp biên khác là`n = 1`. Có chính xác một sự sắp xếp và nó không có cặp thần thánh nào. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp sẽ tạo ra mọi hoán vị và đếm các cặp bên trong nó. Điều này đúng vì mọi ngăn xếp có thể đều được kiểm tra, nhưng nó yêu cầu`n! * n^2`công việc. Đối với các hạn chế tối đa, điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng ta có thể xây dựng hoán vị bằng cách chèn các trang tính theo thứ tự tăng dần của nhãn của chúng. Khi chèn tờ lớn nhất mới`n`, điều duy nhất ảnh hưởng đến các lần chèn trong tương lai là vị trí của trang tính lớn nhất hiện tại. Chúng ta theo dõi xem có bao nhiêu tờ ở dưới tờ lớn nhất. 

Cho phép`dp[r][k]`có nghĩa là trong số các cách sắp xếp của các tờ hiện tại, có`k`cặp đôi thần thánh và chính xác`r`tờ nằm ​​dưới tờ lớn nhất. 

Khi chèn một trang tính lớn nhất mới, giả sử nó được đặt bằng`j`những tờ giấy cũ bên dưới nó. Nếu tờ lớn nhất cũ cũng ở bên dưới nó thì một trong số đó`j`các tờ không thể tạo thành một cặp thần thánh vì các nhãn liền kề bị bỏ qua. Nếu không thì tất cả`j`tờ thấp hơn đóng góp. Tổng tiền tố và hậu tố`r`cho phép tất cả các trạng thái cũ có thể được hợp nhất theo thời gian tuyến tính trên mỗi`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n) | Quá chậm | 
| Lập trình động | O(n^2 * k) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với sự sắp xếp trống. Nó có một trạng thái trong đó tờ lớn nhất không có tờ nào bên dưới và số cặp thần thánh bằng không. 
2. Thêm từng tờ một từ nhãn nhỏ nhất đến nhãn lớn nhất. Đối với mọi trạng thái hiện tại, hãy cân nhắc việc chèn bảng tối đa mới vào mọi khoảng trống có thể có. 
3. Nếu bảng tối đa mới có`j`các trang bên dưới nó, nó góp phần`j`các cặp mới trừ khi trang tối đa trước đó nằm trong số các trang thấp hơn đó. Mức tối đa trước đó là trang tính thấp hơn duy nhất trùng với mức tối đa mới. 
4. Duy trì tổng tiền tố và hậu tố trên số trang dưới mức tối đa trước đó. Phần hậu tố xử lý các trạng thái trong đó mức tối đa cũ nằm dưới mức tối đa mới và phần tiền tố xử lý các trạng thái ở mức trên. 
5. Sau khi xử lý tất cả các trang, tính tổng tất cả các trạng thái có giá trị được yêu cầu là`k`, vì vị trí cuối cùng của trang tính lớn nhất không còn quan trọng nữa. 

Tại sao nó hoạt động: quá trình chèn tạo ra mỗi hoán vị chính xác một lần vì mỗi cách sắp xếp cuối cùng đều có một vị trí duy nhất nơi bảng tối đa mới nhất được chèn vào. Trạng thái được lưu trữ chứa chính xác thông tin cần thiết cho các lần chèn trong tương lai: số lượng trang dưới mức tối đa hiện tại. Không có chi tiết nào khác của thỏa thuận hiện tại có thể ảnh hưởng đến việc đóng góp của bảng tối đa trong tương lai. 

## Giải pháp Python```
Python
```Bảng DP chỉ lưu trữ phạm vi được yêu cầu`k`giá trị vì số lượng lớn hơn không bao giờ có thể ảnh hưởng đến số lượng nhỏ hơn. Mảng tiền tố và hậu tố được xây dựng lại cho mỗi bước chèn để tất cả các vị trí của mức tối đa mới được xử lý mà không cần thêm hệ số`n`. 

Các thao tác dịch chuyển đại diện cho các cặp thiêng liêng mới được tạo ra bởi mức tối đa được chèn vào. các`j`sự thay đổi tương ứng với số lượng tờ dưới, trong khi`j - 1`shift được sử dụng khi mức tối đa trước đó cũng nằm dưới bảng được chèn và phải được loại trừ vì các nhãn liền kề không được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * k) | Mỗi lần chèn xử lý tất cả các vị trí có thể và tất cả các hệ số được lưu trữ | 
| Không gian | O(n * k) | Lớp trạng thái hiện tại lưu trữ các vị trí có thể có của mức tối đa | 

Với`n, k <= 750`, số lượng thao tác đủ nhỏ trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở dưới mức giới hạn.
