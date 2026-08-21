---
title: "CF 102163A - Hasan thẩm phán lười biếng"
description: "Chúng tôi có một tập hợp các đoạn đường ngang và đoạn đường thẳng đứng trên mặt phẳng tọa độ nguyên. Một đoạn ngang được mô tả bởi hai điểm cuối x và tọa độ y cố định của nó. Một đoạn thẳng đứng được mô tả bởi hai điểm cuối y và tọa độ x cố định của nó."
date: "2026-08-21T18:48:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "A"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 2814
verified: false
draft: false
---

[CF 102163A - Hasan thẩm phán lười biếng](https://codeforces.com/problemset/problem/102163/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46 phút 54 giây 
**Đã xác minh:** không 

## Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một tập hợp các đoạn đường ngang và đoạn đường thẳng đứng trên mặt phẳng tọa độ nguyên. Một đoạn ngang được mô tả bởi hai điểm cuối x và tọa độ y cố định của nó. Một đoạn thẳng đứng được mô tả bởi hai điểm cuối y và tọa độ x cố định của nó. 

Việc chọn một đoạn ngang và một đoạn dọc chỉ cho dấu cộng khi chúng giao nhau. Tại điểm giao nhau của chúng, đoạn ngang đóng góp một cánh tay trái và một cánh tay phải, trong khi đoạn dọc đóng góp một cánh tay hướng xuống và một cánh tay hướng lên. Giá trị của dấu cộng này là chiều dài ngắn nhất trong bốn chiều dài cánh tay này. 

Đối với đoạn ngang`[x1, x2]`ở độ cao`y`, giao nhau tại x, đóng góp theo chiều ngang là`min(x - x1, x2 - x)`. 

Đối với đoạn dọc`[y1, y2]`tại tọa độ x, giao nhau ở độ cao y, đóng góp theo chiều dọc là`min(y - y1, y2 - y)`. 

Câu trả lời là giá trị lớn nhất có thể có của giá trị nhỏ nhất của cả bốn đại lượng trên mỗi giao điểm hợp lệ. 

Với tối đa`10^5`ngang và`10^5`phân đoạn dọc, việc kiểm tra từng cặp yêu cầu tối đa`10^10`giao lộ. Cách tiếp cận bậc hai vượt xa những gì giới hạn một giây có thể xử lý được. Tọa độ cũng được giới hạn bởi`10^5`, điều này làm cho cấu trúc dữ liệu logarit trên phạm vi tọa độ trở nên thực tế. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ việc triển khai bất cẩn. Đầu tiên, giao lộ có thể xảy ra chính xác tại điểm cuối. Ví dụ,```

```Hai đoạn cắt nhau tại`(1, 2)`, vậy cánh tay ngắn nhất có chiều dài`0`, và câu trả lời là`0`. Việc triển khai sử dụng các bất đẳng thức nghiêm ngặt thay vì các điều kiện giao nhau bao hàm có thể báo cáo không chính xác rằng không có giao lộ. 

Vấn đề thứ hai là một phân đoạn có thể quá ngắn để hỗ trợ câu trả lời được yêu cầu. Ví dụ,```

```Các đoạn cắt nhau tại`(2, 2)`, nhưng đoạn ngang chỉ có chiều dài`1`, nên không có dấu cộng của độ dài`1`là có thể. Câu trả lời là`0`. Trong quá trình kiểm tra tính khả thi về độ dài`1`, đoạn ngang phải bị loại bỏ vì nó cần tổng chiều dài ít nhất`2`. 

Trường hợp ranh giới thứ ba là điểm cuối đảo ngược. Mặc dù câu lệnh mô tả tọa độ bắt đầu và kết thúc, việc triển khai mạnh mẽ không nên phụ thuộc vào thứ tự của chúng. Ví dụ,```

```Sau khi chuẩn hóa cả hai đoạn, chúng giao nhau tại`(3, 3)`và câu trả lời là`2`. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét mọi phân đoạn ngang và mọi phân đoạn dọc. Đối với mỗi cặp, chúng tôi kiểm tra xem tọa độ x và y của chúng có nằm trong các khoảng tương ứng hay không. Nếu chúng giao nhau, chúng ta tính chiều dài bốn cánh tay và cập nhật câu trả lời. Điều này đúng vì mọi dấu cộng có thể được xác định bởi chính xác một đoạn ngang và một đoạn dọc, vì vậy việc kiểm tra từng cặp không thể bỏ lỡ một điểm tối ưu. 

Vấn đề là số lượng cặp. Với`N = M = 10^5`, có thể có`N * M = 10^10`cặp. Ngay cả việc kiểm tra liên tục cho từng cặp cũng quá chậm, vì vậy chúng ta cần tránh liệt kê các điểm giao nhau. 

Quan sát quan trọng là câu trả lời có thể được kiểm tra thay vì được xây dựng trực tiếp. Giả sử chúng ta hỏi liệu dấu cộng có độ dài ít nhất`d`tồn tại. Đối với đoạn ngang`[x1, x2]`, giao điểm tọa độ x khi đó phải thỏa mãn`x1 + d <= x <= x2 - d`. 

Do đó, đoạn ngang chỉ có thể tham gia thông qua khoảng thời gian rút ngắn của nó.`[x1 + d, x2 - d]`, và nó chỉ có thể sử dụng được khi`x2 - x1 >= 2d`. 

Tương tự, một đoạn dọc`[y1, y2]`chỉ có thể tham gia khi`y1 + d <= y <= y2 - d`. 

Vì vậy, vấn đề cần khắc phục`d`trở thành việc tìm một đoạn ngang có khoảng x rút gọn chứa tọa độ x của một số đoạn dọc có thể sử dụng được, trong khi tọa độ y của đoạn ngang nằm bên trong khoảng y rút gọn của đoạn dọc đó. 

Điều này có thể được xử lý bằng cách quét từ trái sang phải. Sắp xếp các đoạn dọc theo x. Khi chúng ta đạt đến một đoạn thẳng đứng tại x, mọi đoạn ngang có điểm cuối bên trái giảm tối đa là x sẽ hoạt động. Một đoạn ngang vẫn hoạt động cho đến khi điểm cuối bên phải rút gọn của nó trở nên nhỏ hơn x. 

Câu hỏi duy nhất còn lại là làm thế nào để biết liệu một đoạn ngang đang hoạt động có tọa độ y bên trong khoảng y giảm của đoạn dọc hay không. Vì mỗi chiều ngang hoạt động đóng góp một điểm tại tọa độ y của nó, cây Fenwick có thể duy trì số lượng chiều ngang hoạt động tồn tại ở mỗi y. Sau đó, tổng phạm vi sẽ cho chúng ta biết liệu có ít nhất một chiều ngang hoạt động nằm trong khoảng y yêu cầu hay không. 

Vị ngữ là đơn điệu. Nếu dấu cộng của độ dài`d`tồn tại thì dấu cộng có độ dài nhỏ hơn cũng tồn tại. Do đó, chúng ta có thể tìm kiếm nhị phân để có được giá trị khả thi tối đa`d`. 

Lực lượng vũ phu hoạt động vì mọi ứng cử viên đều được kiểm tra rõ ràng, nhưng không thành công vì có quá nhiều cặp. Nhận xét rằng một câu trả lời cố định làm giảm mọi phân đoạn một cách độc lập cho phép chúng ta biến bài toán giao nhau hai chiều thành phép quét một chiều với cây Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM)`|`O(N + M)`| Quá chậm | 
| Tối ưu |`O((N + M) log C log C)`|`O(N + M + C)`| Đã chấp nhận | 

Đây`C <= 10^5`là giới hạn tọa độ. Một logarit xuất phát từ việc tìm kiếm nhị phân qua câu trả lời và logarit kia đến từ các phép toán trên cây Fenwick. 

#Hướng dẫn thuật toán 

1. Chuẩn hóa mọi phân đoạn sao cho điểm cuối đầu tiên của nó không lớn hơn điểm cuối thứ hai. Lưu trữ các đoạn ngang dưới dạng`(x1, x2, y)`và các đoạn thẳng đứng như`(y1, y2, x)`. 
2. Sắp xếp các đoạn ngang theo thứ tự`x1`, sắp xếp chúng một lần theo`x2`và sắp xếp các đoạn thẳng đứng một lần theo`x`. Các lệnh này vẫn hợp lệ cho mọi giá trị tìm kiếm nhị phân vì việc cộng hoặc trừ giống nhau`d`không thay đổi thứ tự. 
3. Tìm kiếm câu trả lời nhị phân`d`. Đối với một ứng cử viên`d`, một đoạn ngang chỉ có thể sử dụng được nếu`x2 - x1 >= 2d`. Có thể có dạng tọa độ x giao nhau của nó`[x1 + d, x2 - d]`. Một đoạn dọc chỉ có thể sử dụng được nếu`y2 - y1 >= 2d`, với tọa độ y có thể giao nhau`[y1 + d, y2 - d]`. 
4. Quét qua các đoạn dọc có thể sử dụng theo thứ tự x tăng dần. Duy trì cây Fenwick được lập chỉ mục bởi y. Khi đoạn thẳng đứng hiện tại có tọa độ x, hãy thêm mọi đoạn ngang có`x1 + d <= x`. Đoạn ngang như vậy có đủ chỗ ở phía bên trái của nó để tạo ra một cánh tay dài ít nhất`d`. 
5. Loại bỏ mọi đoạn ngang có`x2 - d < x`. Đoạn như vậy không còn có thể cung cấp chiều dài cánh tay phải nữa`d`ở thời điểm hiện tại x. Sự nghiêm khắc`<`là cần thiết vì sự bình đẳng có nghĩa là cánh tay phải có chiều dài chính xác`d`, hợp lệ. 
6. Truy vấn cây Fenwick`[y1 + d, y2 - d]`. Tổng phạm vi dương có nghĩa là một số phân đoạn ngang đang hoạt động có tọa độ y trong phạm vi dọc hợp lệ. Đoạn ngang và đoạn dọc hiện tại tạo thành một dấu cộng có ít nhất bốn cánh tay`d`, vậy là kiểm tra thành công. 
7. Nếu kiểm tra thành công, di chuyển giới hạn dưới của tìm kiếm nhị phân lên trên. Ngược lại, di chuyển giới hạn trên xuống dưới. Giá trị thành công lớn nhất là câu trả lời. 

### Tại sao nó hoạt động 

Đối với một cố định`d`, một đoạn ngang được biểu diễn chính xác trong quá trình quét trong khi tọa độ x của nó có thể được chọn sao cho cả hai nhánh ngang có chiều dài ít nhất`d`. Do đó, một đoạn ngang hoạt động tương đương với điều kiện`x1 + d <= x <= x2 - d`. 

Tại một đoạn thẳng đứng có tọa độ x, cây Fenwick chứa chính xác tọa độ y của tất cả các đoạn ngang thỏa mãn điều kiện nằm ngang đó. Truy vấn`[y1 + d, y2 - d]`thực thi thêm cả hai điều kiện cánh tay dọc. Do đó, truy vấn thành công chính xác khi tồn tại một giao điểm có ít nhất bốn nhánh`d`. 

Vị từ khả thi là đơn điệu, bởi vì giảm`d`chỉ thư giãn khoảng cách cần thiết. Do đó, tìm kiếm nhị phân tìm thấy độ dài khả thi lớn nhất. 

#Giải pháp Python```
Python
```Giai đoạn đầu vào bình thường hóa các điểm cuối trước tiên. Điều này tránh việc mọi thao tác sau này phải xử lý cả hai hướng có thể. 

Ba mảng được sắp xếp là cốt lõi của quá trình quét. Sắp xếp theo`x1`cho phép thuật toán thêm các chiều ngang theo thứ tự chính xác mà chúng đủ điều kiện. Sắp xếp theo`x2`cho phép nó loại bỏ chúng khi điểm cuối bên phải của chúng trở nên quá gần với tọa độ x hiện tại. Mảng dọc được sắp xếp theo x vì quá trình quét tự di chuyển từ trái sang phải. 

Đối với một ứng cử viên`d`, điều kiện`x1 + d <= x`xác định việc chèn. điều kiện`x2 - d < x`quyết định việc loại bỏ. Sự so sánh thứ hai là nghiêm ngặt bởi vì`x2 - d == x`cung cấp một cánh tay phải chính xác`d`, phải duy trì hiệu lực. 

Cây Fenwick lưu trữ số lượng chứ không phải booleans. Nhiều đoạn ngang có thể có cùng tọa độ y, do đó, việc xóa một đoạn không được vô tình xóa đoạn khác. Một số đếm xử lý các giá trị y trùng khớp một cách tự nhiên. 

Truy vấn phạm vi sử dụng`prefix(high_y) - prefix(low_y - 1)`, 

đó là tổng phạm vi Fenwick tiêu chuẩn. Điều này cũng xử lý trường hợp khoảng y hợp lệ chứa chính xác một tọa độ. 

Số nguyên Python không bị tràn, vì vậy tất cả số học tọa độ đều an toàn. Tọa độ liên quan lớn nhất chỉ là`10^5`, trong khi phép nhân tìm kiếm nhị phân`2 * d`cũng bé nhỏ. 

# Ví dụ đã hoạt động 

## Mẫu 1 

Đầu vào chứa các đoạn ngang```

```và các đoạn dọc```

```Dấu vết sau đây cho thấy các bước kiểm tra tính khả thi mang tính quyết định. 

|`d`| Phạm vi ngang có thể sử dụng | Phạm vi dọc có thể sử dụng | Kết quả | 
| --- | --- | --- | --- | 
|`3`|`[1,5]`trở thành`[4,2]`,`[2,4]`trở thành`[]`,`[6,12]`trở thành`[9,9]`| Chiều dọc đầu tiên trở thành`[]`, thứ hai trở thành`[]`| Sai | 
|`1`|`[1,5]`->`[2,4]`,`[2,4]`->`[3,3]`,`[6,12]`->`[7,11]`|`[1,5]`->`[2,4]`,`[6,9]`->`[7,8]`| Đúng | 
|`2`|`[1,5]`->`[3,3]`,`[2,4]`->`[4,2]`,`[6,12]`->`[8,10]`|`[1,5]`->`[3,3]`,`[6,9]`->`[8,7]`| Đúng | 

Vì`d = 2`, đoạn ngang`[1,5]`chỉ có thể được sử dụng ở tọa độ x`3`. Phân đoạn dọc`[1,5]`tại tọa độ x`3`chỉ có thể được sử dụng ở tọa độ y`3`. Giao điểm của họ là`(3,3)`, và cả bốn cánh tay đều có chiều dài ít nhất`2`. Giá trị lớn hơn là không thể, vì vậy câu trả lời là`2`. 

## Xây dựng ví dụ 2 

Hãy xem xét```

```Vì`d = 2`, khoảng cách ngang giảm là`[3,5]`tại y=4 và`[5,3]`đối với chiều ngang thứ hai, do đó chỉ chiều ngang đầu tiên vẫn có thể sử dụng được. Khoảng thời gian dọc giảm là`[4,6]`tại x=5 và`[3,4]`tại x=3. 

| Dọc | x | Giá trị y ngang đang hoạt động | Phạm vi y bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | 
|`[1,6]`tại x=3 | 3 | không |`[3,4]`| không | 
|`[2,8]`tại x=5 | 5 |`{4}`|`[4,6]`| vâng | 

Đường thẳng đứng thứ hai cắt đường ngang thứ nhất tại`(5,4)`. Chiều dài bốn cánh tay của nó là`4`,`2`,`2`, Và`4`, vậy dấu cộng có độ dài`2`. 

Ví dụ này chứng tỏ tại sao cây Fenwick cần duy trì các chiều ngang đang hoạt động thay vì chỉ kiểm tra xem các đoạn có giao nhau ở đâu đó hay không. Giao lộ bắt buộc phải rời khỏi ít nhất`d`đơn vị ở mọi phía. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((N + M) log C log C)`| Tìm kiếm nhị phân thực hiện`O(log C)`kiểm tra và mọi kiểm tra đều thực hiện`O(N + M)`Hoạt động của Fenwick, mỗi lần tham gia`O(log C)`| 
| Không gian |`O(N + M + C)`| Ba bộ sưu tập phân đoạn được sắp xếp và một cây Fenwick trên phạm vi tọa độ | 

Với`C <= 10^5`, tìm kiếm nhị phân cần tối đa khoảng 17 lần lặp. Mỗi phân đoạn vào và ra khỏi cấu trúc Fenwick nhiều nhất một lần trong quá trình kiểm tra, trong khi mỗi phân đoạn dọc gây ra một số lượng truy vấn tiền tố Fenwick không đổi. Độ phức tạp thu được là logarit trong phạm vi tọa độ trên đầu quét tuyến tính của các phân đoạn, phù hợp với các giới hạn đã cho. 

# Trường hợp thử nghiệm```
Python
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`2`| Giao điểm thông thường và tìm kiếm nhị phân | 
| Một lần chạm ngang và một lần chạm dọc tại điểm cuối |`0`| Ranh giới bao gồm và cánh tay có chiều dài bằng không | 
|`[1,5]`đi qua`[1,5]`tập trung |`2`| Tối đa chính xác khi mỗi cánh tay có chiều dài`2`| 
| Điểm cuối đảo ngược |`2`| Chuẩn hóa điểm cuối | 
| Đoạn có giao điểm hình học nhưng không có dấu cộng dương |`0`| Từ chối chiều dài cánh tay không đủ | 
|`100000`chiều ngang và chiều dài đầy đủ |`49999`| Kích thước và hiệu suất đầu vào tối đa | 

# Vỏ cạnh 

Trường hợp giao điểm cuối```

```được xử lý bởi`d = 0`. Chiều ngang bắt đầu hoạt động khi`x1 + 0 <= x`, vậy nó hoạt động tại x=`1`. Ranh giới bên phải của nó không bị xóa cho đến khi`x2 < x`. Tại tọa độ x của phương thẳng đứng`1`, truy vấn Fenwick bao gồm y=`2`, do đó giao điểm được tìm thấy và tìm kiếm nhị phân tiếp tục`0`như câu trả lời. Điều kiện loại bỏ nghiêm ngặt là điều kiện duy trì giao điểm điểm cuối. 

Trường hợp không đủ độ dài```

```minh họa`x2 - x1 >= 2d`tình trạng. Vì`d = 1`, chiều ngang có chiều dài`1`, nhỏ hơn`2`, vì vậy nó không bao giờ được đưa vào cây Fenwick. Mặc dù hai đoạn thẳng cắt nhau nhưng không có bốn cạnh nào dài`1`có thể tồn tại. Việc kiểm tra không thành công và câu trả lời vẫn còn`0`. 

Trường hợp điểm cuối đảo ngược```

```được chuẩn hóa theo chiều ngang`[1,5]`và dọc`[1,5]`. Tại`d = 2`, cả hai khoảng giảm đều sụp đổ để phối hợp`3`, tạo ra giao điểm`(3,3)`. Câu trả lời là`2`. Nếu không chuẩn hóa, các so sánh như`x1 + d <= x`sẽ vô nghĩa đối với biểu diễn đảo ngược. 

Cuối cùng, nhiều đoạn ngang có thể có cùng tọa độ y. Cây Fenwick lưu trữ số đếm ở mỗi tọa độ thay vì trạng thái boolean. Nếu hai phân đoạn hoạt động đều nằm ở y=`7`, chèn chúng sẽ tạo ra số lượng`2`và loại bỏ một cái sẽ tạo ra số lượng`1`. Đoạn còn lại vẫn được thể hiện chính xác, do đó tọa độ chồng chéo không làm hỏng quá trình quét.
