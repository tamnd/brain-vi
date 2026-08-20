---
title: "CF 102163A - Hasan thẩm phán lười biếng"
description: "Chúng ta có các đoạn đường ngang và các đoạn đường thẳng đứng trên mặt phẳng tọa độ nguyên. Dấu cộng hợp lệ được hình thành bằng cách chọn một đoạn ngang và một đoạn thẳng cắt nhau tại điểm C."
date: "2026-08-20T14:18:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "A"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 1454
verified: false
draft: false
---

[CF 102163A - Hasan thẩm phán lười biếng](https://codeforces.com/problemset/problem/102163/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các đoạn đường ngang và các đoạn đường thẳng đứng trên mặt phẳng tọa độ nguyên. Dấu cộng hợp lệ được hình thành bằng cách chọn một đoạn ngang và một đoạn thẳng cắt nhau tại điểm C. Bốn nhánh của dấu cộng là khoảng cách từ C đến hai điểm cuối của đoạn ngang và hai điểm cuối của đoạn thẳng đứng. 

Đối với đoạn ngang [x 1 ​ ,x 2 ​ ] và đoạn thẳng đứng [y 1 ​ ,y 2 ​ ], cắt nhau tại (x,y), giá trị của nó là 

phút(x−x 1 ​ , x 2 ​ −x, y−y 1 ​ , y 2 ​ −y). 

Chúng ta cần giá trị tối đa trên mọi cặp giao nhau có thể. Đầu vào chứa T trường hợp thử nghiệm độc lập, theo sau là các phân đoạn ngang và sau đó là các phân đoạn dọc. Tọa độ tối đa là 10 5. 

Giải pháp hấp dẫn là kiểm tra mọi phân đoạn ngang và dọc. Với N,M<10 5, điều đó có nghĩa là có tới 10 10 cặp trong một trường hợp thử nghiệm. Giới hạn một giây làm cho cách tiếp cận bậc hai như vậy là không thể. Chúng ta cần một cái gì đó gần với O((N+M)logC), trong đó C<10 5 là phạm vi tọa độ hoặc ít nhất là một hệ số logarit nhỏ hơn. 

Có một số trường hợp ranh giới quan trọng. Một đoạn có thể quá ngắn để chứa dấu cộng có kích thước dương. Ví dụ,```

```Hai đoạn duy nhất cắt nhau, nhưng mọi cánh tay đều có độ dài bằng 0, vì vậy câu trả lời là`0`. Một giải pháp giả định câu trả lời luôn là tích cực sẽ sai. 

Giao lộ cũng có thể xảy ra chính xác tại một điểm cuối. Ví dụ,```

```Các đoạn gặp nhau tại (1,2) nhưng đoạn ngang không có nhánh bên trái nút giao. Câu trả lời là`0`. Khi kiểm tra độ dài dự kiến ​​d, giao điểm phải được phép ở ranh giới của các đoạn được cắt bớt, do đó điều kiện nằm ngang được bao gồm. 

Tọa độ trùng lặp là một nguồn sai lầm dễ xảy ra khác. Ví dụ,```

```Câu trả lời là`2`. Hai phân đoạn ngang có thể có tọa độ y giống hệt nhau và cấu trúc dữ liệu phải tính cả hai một cách độc lập. Việc sử dụng boolean thay vì tần số có thể gây ra thao tác xóa không chính xác khi một trong các phân đoạn trùng lặp ngừng hoạt động. 

Cuối cùng, điểm cuối của phân đoạn ngang và dọc không nhất thiết phải được đưa ra theo thứ tự tăng dần theo cách diễn đạt của câu lệnh. Việc triển khai mạnh mẽ sẽ chuẩn hóa mọi phân đoạn sao cho tọa độ đầu tiên của nó không lớn hơn tọa độ thứ hai. 

## Phương pháp tiếp cận 

Giải pháp brute-force được rút ra trực tiếp từ hình học. Đối với mỗi phân đoạn ngang và mỗi phân đoạn dọc, chúng tôi kiểm tra xem phạm vi tọa độ của chúng có trùng nhau theo cách yêu cầu hay không. Nếu chúng cắt nhau tại (x,y), chúng ta tính độ dài bốn cánh tay và cập nhật câu trả lời. 

Điều này đúng vì mọi dấu cộng có thể được xác định bởi chính xác một đoạn ngang và một đoạn dọc, vì vậy việc kiểm tra từng cặp không thể bỏ sót một ứng cử viên nào. Vấn đề là số lượng cặp NM. Với N=M=10 5, có thể có 10 10 cặp trong một trường hợp thử nghiệm, vượt xa giới hạn thời gian cho phép. 

Quan sát hữu ích là chúng ta có thể biến vấn đề tối ưu hóa thành vấn đề quyết định. Giả sử chúng ta hỏi liệu dấu cộng có kích thước ít nhất là d có tồn tại hay không. 

Đối với đoạn nằm ngang [x 1​ ,x 2​ ], giao điểm phải cách cả hai điểm cuối nằm ngang ít nhất là d đơn vị. Do đó tọa độ x của nó phải thỏa mãn 

x 1 ​ +d<x<x 2 ​ −d. 

Khoảng này khác rỗng chính xác khi 

x 2 ​ −x 1 ​ ≥2d. 

Tương tự, đối với đoạn thẳng đứng [y 1​ ,y 2​ ], một giao lộ phù hợp phải thỏa mãn 

y 1 ​ +d<y<y 2​ −d, 

đòi hỏi y 2 ​ −y 1 ​ ≥2d. 

Vì vậy, sau khi cố định d, mọi đoạn ngang đủ dài sẽ trở thành một khoảng hoạt động theo hướng x, mang tọa độ y của nó. Một đoạn dọc trở thành một truy vấn tại tọa độ x của nó, hỏi xem liệu tọa độ y ngang đang hoạt động nào đó có nằm bên trong hay không 

[y 1 ​ +d, y 2​ −d]. 

Đây chính xác là một vấn đề về đường quét ngoại tuyến. Sắp xếp các đoạn dọc theo x. Khi chúng ta di chuyển từ trái sang phải, hãy chèn một đoạn ngang khi x đạt x 1 ​ +d và xóa nó sau x 2 ​ −d. Tại x của phân đoạn dọc, các phân đoạn ngang đang hoạt động chính xác là những phân đoạn có khoảng ngang được cắt bớt chứa x đó. 

Cây Fenwick trên tọa độ y lưu trữ số lượng đoạn ngang hoạt động tồn tại ở mỗi y. Tổng phạm vi cho chúng ta biết liệu có ít nhất một phân đoạn ngang đang hoạt động có tọa độ y trong phạm vi được cắt bớt của phân đoạn dọc hay không. 

Điều kiện khả thi là đơn điệu. Nếu tồn tại dấu cộng có kích thước d thì giao điểm tương tự cũng cho dấu cộng ở mọi kích thước nhỏ hơn. Điều đó làm cho việc tìm kiếm nhị phân trên d có thể thực hiện được. 

Việc triển khai có thể làm cho việc quét rẻ hơn so với việc sắp xếp các sự kiện mới trong mỗi lần lặp tìm kiếm nhị phân. Thứ tự của các đoạn ngang x 1 ​ không bao giờ thay đổi khi chúng ta thêm cùng một d vào mọi x 1 ​ và thứ tự theo x 2 ​ không bao giờ thay đổi khi chúng ta trừ đi cùng một d. Chúng tôi sắp xếp các đơn đặt hàng này một lần và sử dụng lại chúng cho mỗi lần kiểm tra tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N+M) | Quá chậm | 
| Tối ưu | O((N+M)logClogC) | O(N+M+C) | Đã chấp nhận | 

Ở đây C<10 5 là tọa độ cực đại. Logarit đầu tiên đến từ các phép toán Fenwick và logarit thứ hai đến từ tìm kiếm nhị phân. 

## Hướng dẫn thuật toán

1. Chuẩn hóa mọi đoạn ngang sao cho x 1 ​ ≤x 2 ​ và mọi đoạn dọc sao cho y 1 ​ ≤y 2 ​. Tính câu trả lời lớn nhất có thể từ nửa độ dài của mỗi đoạn. Điểm cộng có kích thước d cần tổng độ dài ít nhất là 2d trên cả hai đoạn đã chọn, do đó không có câu trả lời nào có thể vượt quá nửa độ dài tối đa. 
2. Sắp xếp các đoạn dọc theo tọa độ x của chúng. Trong quá trình kiểm tra tính khả thi, chúng sẽ được xử lý từ trái sang phải, khớp với hướng quét. 
3. Sắp xếp các đoạn ngang một lần theo x 1 ​ và một lần theo x 2 ​. Đối với một ứng cử viên cố định d, một đoạn ngang bắt đầu sử dụng được ở x 1 ​ +d và ngừng sử dụng được sau x 2 ​ −d. Việc cộng hoặc trừ cùng một d không làm thay đổi thứ tự sắp xếp, vì vậy những thứ tự này có thể được sử dụng lại trong tất cả các lần kiểm tra. 
4. Để kiểm tra ứng cử viên d, bỏ qua mọi đoạn ngang có x 2 ​ −x 1 ​ <2d. Đoạn như vậy không thể cung cấp cả hai nhánh ngang có chiều dài d. Tương tự, bỏ qua mọi đoạn thẳng đứng có y 2 ​ −y 1 ​ <2d. 
5. Quét qua các đoạn thẳng đứng theo chiều tăng x. Duy trì cây Fenwick được lập chỉ mục bởi y. Đối với mọi đoạn ngang đủ điều kiện có x 1 ​ +d<x, hãy thêm một đoạn tại tọa độ y của nó. Đây chính xác là những đoạn ngang mà cánh tay trái của nó có thể chứa giao lộ hiện tại. 
6. Loại bỏ mọi đoạn ngang đủ điều kiện thỏa mãn x 2 ​ −d<x. Sự bất bình đẳng nghiêm ngặt là có chủ ý. Khi x=x 2 ​ −d, cánh tay phải có độ dài chính xác là d nên đoạn ngang vẫn phải hoạt động tại tọa độ đó. 
7. Đối với một đoạn thẳng đứng đủ điều kiện, hãy truy vấn cây Fenwick 

[y 1 ​ +d, y 2​ −d]. 

Nếu phạm vi chứa ít nhất một đoạn ngang đang hoạt động thì d hiện tại là khả thi. Các đoạn thẳng ngang và dọc tương ứng cắt nhau tại một điểm có độ dài cả bốn cánh tay ít nhất là d. 

1. Tìm kiếm nhị phân khả thi lớn nhất d. Nếu kiểm tra thành công, hãy di chuyển giới hạn dưới lên trên. Nếu không thì di chuyển giới hạn trên xuống dưới. 

### Tại sao nó hoạt động 

Đối với một d cố định, một đoạn ngang hoạt động ở chính xác các tọa độ x thỏa mãn x 1 ​ +d<x<x 2 ​ −d. Quá trình quét sẽ chèn nó vào tọa độ đầu tiên như vậy và loại bỏ nó ngay sau tọa độ cuối cùng như vậy. Do đó, tại mỗi x dọc được xử lý, cây Fenwick chứa chính xác tọa độ y của các đoạn ngang có thể hỗ trợ cộng kích thước d tại x đó. 

Truy vấn Fenwick thành công chính xác khi một trong các đoạn ngang đó có y bên trong [y 1 ​ +d,y 2 ​ −d]. Điều kiện đó cho ít nhất d đơn vị diện tích thẳng đứng ở cả hai phía của giao lộ. Cùng với điều kiện kích hoạt theo chiều ngang, cả bốn cánh tay đều có ít nhất d. Vì vậy việc kiểm tra tính khả thi là chính xác. 

Vì tính khả thi của d ngụ ý tính khả thi đối với mọi giá trị nhỏ hơn nên tìm kiếm nhị phân trả về kích thước cộng lớn nhất có thể. 

## Giải pháp Python```
Python
```Giai đoạn đầu vào lưu trữ từng phân đoạn sau khi chuẩn hóa điểm cuối của nó. Câu trả lời tối đa có thể được tính toán cùng lúc, điều này mang lại cho tìm kiếm nhị phân một giới hạn trên chặt chẽ. 

Hai bản sao được sắp xếp của các phân đoạn ngang là tối ưu hóa tiền xử lý chính.`by_left`kiểm soát việc chèn vào, trong khi`by_right`kiểm soát việc loại bỏ. Lệnh của họ có giá trị cho mọi ứng cử viên`d`, bởi vì việc cộng cùng một giá trị cho tất cả các điểm cuối bên trái và trừ đi cùng một giá trị từ tất cả các điểm cuối bên phải sẽ duy trì thứ tự tương đối của chúng. 

Mỗi cuộc gọi đến`check`tạo ra một cây Fenwick tươi mới. Điều này tránh được vấn đề dọn dẹp tinh vi có thể xảy ra nếu quá trình kiểm tra thành công thoát ra sớm trong khi vẫn để lại các tần số cũ trong cây. Một cây mới cũng giữ cho lập luận về tính đúng đắn trở nên đơn giản. 

Điều kiện chèn sử dụng`x1 + d <= x`. Điều kiện loại bỏ sử dụng`x2 - d < x`. Những bất đẳng thức này làm cho khoảng hoạt động bao hàm chính xác ở cả hai đầu. Đảo ngược một trong hai ranh giới sẽ từ chối không chính xác một dấu cộng có nhánh có chiều dài chính xác`d`. 

Cây Fenwick sử dụng tọa độ y thực tế làm chỉ số của nó. Vì tọa độ tối đa là 10 5 nên không cần nén tọa độ. Số nguyên Python cũng không gặp vấn đề tràn đối với số lượng Fenwick. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```

```Xét d=2. Ba đoạn ngang có độ dài 4,2,6, vì vậy đoạn thứ nhất và thứ ba có thể hỗ trợ giá trị này. Hai đoạn thẳng đứng có độ dài 4,3 nên cả hai đều là ứng cử viên. 

Quá trình quét hoạt động như sau. 

| Đoạn dọc | x | Đã chèn giá trị y ngang | Đã xóa giá trị y ngang | Phạm vi truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| [1,5] | 3 | y=3 từ [1,5] | không | [3,3] | tìm thấy | 
| [6,9] | 2 | chưa đạt theo thứ tự x | không | [8,7] | không cần thiết | 

Đoạn thẳng đứng đầu tiên tại x=3 cắt đoạn thẳng từ x=1 đến x=5, cũng tại y=3. Chiều dài bốn cánh của nó là 2,2,2,2 nên d=2 là khả thi. 

Việc thử d=3 không thành công vì đoạn ngang đầu tiên chỉ có độ dài 4, trong khi đoạn ngang thứ ba ở y=6 và không giao với đoạn dọc liên quan có đủ chỗ. Như vậy câu trả lời là`2`. 

### Xây dựng ví dụ 2 

Hãy xem xét```

```Đoạn ngang là [1,9] tại y=5 và đoạn dọc là [3,7] tại x=5. Giao điểm của họ là (5,5). 

| Candidate d | Khoảng cắt ngang | Khoảng thời gian cắt dọc | Intersection | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | [2,8] | [4,6] | (5,5) | vâng | 
| 2 | [3,7] | [5,5] | (5,5) | vâng | 
| 3 | [4,6] | [6,4] | trống | không | 

The maximum value is`2`. Dấu vết này giải thích tại sao khoảng dọc phải được cắt bớt ở cả hai đầu và tại sao một đoạn có tổng chiều dài chính xác là 2d vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M)logClogC) | Mỗi kiểm tra tìm kiếm nhị phân thực hiện các phép toán Fenwick O(N+M), mỗi phép lấy O(logC) và có các giá trị ứng cử viên O(logC). | 
| Không gian | O(N+M+C) | Các phân đoạn, hai thứ tự ngang được sắp xếp, các phân đoạn dọc được sắp xếp và một cây Fenwick được lưu trữ. | 

Với C<10 5, tìm kiếm nhị phân cần tối đa khoảng 17 lần lặp. Giải pháp tránh việc liệt kê 10 10 cặp lực lượng vũ phu và chỉ sử dụng bộ lưu trữ có kích thước tuyến tính, phù hợp với giới hạn bộ nhớ đã nêu. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solution(data: str) -> str:    it = iter(data.split())    t = int(next(it))    answers = []
    for _ in range(t):        n = int(next(it))        m = int(next(it))
        horizontal = []        vertical = []        hi = 0        max_coord = 0
        for _ in range(n):            x1 = int(next(it))            x2 = int(next(it))            y = int(next(it))            if x1 > x2:                x1, x2 = x2, x1            horizontal.append((x1, x2, y))            hi = max(hi, (x2 - x1) // 2)            max_coord = max(max_coord, x2, y)
        for _ in range(m):            y1 = int(next(it))            y2 = int(next(it))            x = int(next(it))            if y1 > y2:                y1, y2 = y2, y1            vertical.append((y1, y2, x))            hi = max(hi, (y2 - y1) // 2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`2`| Ví dụ chính thức và cách xử lý giao lộ thông thường | 
| Phân đoạn ngang và dọc một điểm |`0`| Phân đoạn có kích thước tối thiểu và câu trả lời bằng 0 | 
|`[1,9]`với`[3,7]`|`2`| Tìm kiếm nhị phân và tối ưu tập trung chính xác | 
|`[1,3]`có đoạn thẳng đứng tại x=1 |`0`| Nút giao tại điểm cuối | 
| Hai đoạn ngang giống hệt nhau |`2`| Tọa độ y trùng lặp và tần số Fenwick | 
| 10 5 đoạn dài giống hệt nhau |`49999`| Kích thước và hiệu suất đầu vào tối đa | 

## Vỏ cạnh 

Đối với một đoạn có độ dài bằng 0, hãy xem xét```
11 11 1 11 1 1
```Giới hạn trên ban đầu bằng 0 vì cả hai đoạn đều có nửa độ dài bằng 0. Tìm kiếm nhị phân ngay lập tức trả về số 0. Thuật toán không bao giờ cố gắng tạo ra nhánh dương từ một đoạn không thể chứa nhánh dương. 

Đối với giao lộ điểm cuối, hãy xem xét```
11 11 3 22 4 1
```Đoạn ngang chỉ có thể hỗ trợ d=1 tại x=2, nhưng đường thẳng đứng tại x=1. Đối với d=1, khoảng cắt ngang là [2,2], do đó quá trình quét không bao giờ kích hoạt đoạn ngang đó khi xử lý x=1. Việc kiểm tra không thành công và câu trả lời vẫn là 0. 

Đối với tọa độ trùng lặp, hãy xem xét```
12 11 5 31 5 31 5 3
```Đối với d=2, các phân đoạn ngang hoạt động từ x=3 đến x=3 và phân đoạn dọc tại x=3 truy vấn y=3. Cây Fenwick chứa số lượng hai tại tọa độ đó. Nếu một phân đoạn ngang bị xóa, số đếm sẽ trở thành một chứ không phải 0, đó là lý do tại sao quá trình triển khai lưu trữ số đếm thay vì boolean. 

Đối với các đoạn có độ dài chính xác là 2d, khoảng được cắt bớt bao gồm một tọa độ duy nhất. Coi như```
11 11 5 31 5 3
```Với d=2, khoảng cắt ngang là [3,3] và khoảng cắt dọc là [3,3]. Cả hai phân đoạn đều hoạt động ở tọa độ 3, vì vậy câu trả lời là`2`. Điều kiện chèn bao gồm và điều kiện loại bỏ nghiêm ngặt là những gì bảo toàn trường hợp biên hợp lệ này. 

Đối với các điểm cuối đảo ngược, hãy xem xét```
11 19 1 57 3 5
```Quá trình chuẩn hóa thay đổi các phân đoạn thành [1,9] và [3,7], đưa ra dấu cộng ở giữa giống như ví dụ trước và câu trả lời về`2`. Nếu không chuẩn hóa, việc trừ trực tiếp các điểm cuối có thể tạo ra độ dài âm và giới hạn tìm kiếm nhị phân không hợp lệ.
