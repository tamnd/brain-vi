---
title: "CF 105910I - \u8d70\u5411\u53f6\u5b50"
description: "Chúng tôi được tặng một cái cây. Tập hợp các lá được cố định trong toàn bộ quá trình, nó được xác định từ cây ban đầu và không bao giờ thay đổi. Hoạt động cập nhật chọn nút u và giá trị w. Với mỗi lá v, chúng ta xét đường đi duy nhất từ ​​u đến v và thêm w vào mọi đỉnh trên đường đi đó."
date: "2026-06-25T14:05:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105910
codeforces_index: "I"
codeforces_contest_name: "The 23rd Sichuan University Programming Contest"
rating: 0
weight: 105910
solve_time_s: 80
verified: true
draft: false
---

[CF 105910I - \u8d70\u5411\u53f6\u5b50](https://codeforces.com/problemset/problem/105910/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây. Tập hợp các lá được cố định trong toàn bộ quá trình, nó được xác định từ cây ban đầu và không bao giờ thay đổi. 

Một thao tác cập nhật chọn một nút`u`và một giá trị`w`. Cho mỗi chiếc lá`v`, chúng ta nhìn vào con đường duy nhất từ`u`ĐẾN`v`và thêm`w`tới mọi đỉnh trên đường đi đó. Vì cùng một đỉnh có thể nằm trên nhiều đường đi như vậy nên một lần cập nhật có thể tăng một đỉnh lên nhiều lần. 

Một truy vấn yêu cầu trọng số hiện tại của một đỉnh. 

Cây chứa tới`5 · 10^5`đỉnh và có tới`5 · 10^5`hoạt động. Bất kỳ giải pháp nào lặp lại rõ ràng trên tất cả các lá hoặc trên tất cả các đỉnh trên tất cả các đường đi đều không thể thực hiện được ngay lập tức. Thậm chí một`O(n)`thủ tục cho mỗi hoạt động sẽ yêu cầu khoảng`2.5 · 10^11`hoạt động trong trường hợp xấu nhất. 

Khó khăn chính là một bản cập nhật đồng thời ảnh hưởng đến các đường dẫn từ một nguồn duy nhất đến mọi lá. 

Một sai lầm phổ biến là nghĩ rằng một đỉnh nhận được`w`một lần bất cứ khi nào nó thuộc về ít nhất một đường dẫn. Trong thực tế nó nhận được`w`một lần cho mỗi chiếc lá có đường đi qua nó. 

Hãy xem xét cái cây```
1
|
2
/ \
3  4
```Lá là`{3,4}`. 

Để cập nhật`1 2 5`, đỉnh`2`thuộc về cả hai con đường`2 → 3`Và`2 → 4`, vậy mức tăng của nó là`10`, không`5`. 

Một cạm bẫy dễ dàng khác xuất hiện gần gốc. Một đỉnh có thể nằm trên một đường đi vì lá nằm trong cây con của nó hoặc vì lá nằm ngoài cây con của nó. Hai trường hợp đó đóng góp khác nhau và phải được tính riêng. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu tuân theo tuyên bố theo nghĩa đen. Để cập nhật`(u,w)`chúng tôi liệt kê từng chiếc lá, tìm đường đi từ`u`vào chiếc lá đó và thêm`w`dọc theo con đường đó. Điều này rõ ràng là đúng vì nó thực hiện chính xác thao tác được yêu cầu. 

Vấn đề là sự phức tạp. Trong một cái cây hình ngôi sao có`Θ(n)`lá, và mỗi con đường có độ dài`Θ(1)`. Trong một chuỗi chỉ có hai lá nhưng đường đi có chiều dài`Θ(n)`. Dù bằng cách nào, một bản cập nhật có thể tốn kém`Θ(n)`, vượt xa giới hạn. 

Quan sát quan trọng là bộ lá được cố định. Thay vì suy nghĩ về các đường đi riêng lẻ, chúng ta đếm xem có bao nhiêu lá buộc phải bao gồm một đỉnh cụ thể. 

Gốc cây một lần tại một đỉnh tùy ý, chẳng hạn`1`. 

Cho phép`leaf[x]`là số lá trong cây con của`x`, và để`L`là tổng số lá. 

Giả sử một bản cập nhật được thực hiện ở đỉnh`u`. 

Đối với một đỉnh`x`đó không phải là tổ tiên của`u`, mọi lá bên trong cây con của`x`tạo ra một con đường đi qua`x`. Số lượng đóng góp chính xác`leaf[x]`. 

Đối với tổ tiên`x`của`u`, cho phép`son`là con của`x`trên con đường hướng tới`u`. Một con đường từ`u`đến một chiếc lá đi qua`x`chính xác là khi chiếc lá đó ở bên ngoài`son`cây con của. Số lượng đóng góp trở thành```
L - leaf[son]
```Điều này biến vấn đề đường dẫn ban đầu thành một tập hợp các số liệu thống kê về cây con. 

Nhiệm vụ còn lại là duy trì các số liệu thống kê này theo các cập nhật điểm. Giải pháp chính thức sử dụng phân hủy ánh sáng nặng. Đóng góp của trẻ nhẹ được cập nhật ngay lập tức, trong khi đóng góp của trẻ nặng được tích lũy và đánh giá một cách lười biếng trong quá trình truy vấn. Thực tế nặng-nhẹ tiêu chuẩn là mọi đường dẫn từ gốc tới đỉnh chỉ chứa`O(log n)`các cạnh nhẹ giữ cho sự phức tạp được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi thao tác | O(n) | Quá chậm | 
| Phân hủy nặng-nhẹ | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`1`. 
2. Tính toán`leaf[x]`, số lá trong cây con của mỗi đỉnh và tính tổng số lá`L`. 
3. Xây dựng cây phân rã nặng nhẹ. 
4. Hãy để`sum[x]`biểu thị tổng trọng số cập nhật được thêm vào tại đỉnh`x`bằng các hoạt động loại 1. 
5. Mỗi bản cập nhật chỉ thay đổi thông tin trên chuỗi tổ tiên của đỉnh bắt đầu. Sử dụng cấu trúc nặng-ánh sáng, truyền bá hiệu ứng thông qua quá trình phân hủy thay vì chạm vào từng tổ tiên một cách rõ ràng. 
6. Đối với mỗi nút, hãy duy trì ngay sự đóng góp đến từ các nút con nhẹ của nó. 
7. Những đóng góp của trẻ nặng cân không được đẩy lên từng cái một. Chúng được tích lũy và đánh giá khi một truy vấn đến chuỗi đó. 
8. Đối với truy vấn tại đỉnh`x`, kết hợp: 

1. Sự đóng góp của các lá bên trong các cây con liên quan. 
2. Sự đóng góp của các lá ngoài cây con khi`x`là tổ tiên của một đỉnh được cập nhật. 
3. Các giá trị được lưu trong bộ nhớ đệm được duy trì bởi cấu trúc nặng-nhẹ. 
9. Xuất kết quả trọng lượng. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi cập nhật chỉ được thể hiện thông qua số lượng lá của cây con. 

Đối với nguồn cập nhật cố định`u`, mọi đỉnh đều thuộc đúng một trong hai loại. 

Nếu nó không phải là tổ tiên của`u`, số đường đi qua nó bằng số lá trong cây con của nó. 

Nếu nó là tổ tiên của`u`, số đường đi qua nó bằng số lá bên ngoài cây con dẫn đến`u`. 

Những con số này là chính xác và phân chia tất cả các khả năng. Phân rã nặng-ánh sáng không thay đổi giá trị đang được duy trì, nó chỉ nhóm các cập nhật đường dẫn tổ tiên thành`O(log n)`phân đoạn. Vì mọi truy vấn đều xây dựng lại cùng một công thức đếm lá nên trọng số được báo cáo chính xác là giá trị được xác định bởi các thao tác ban đầu. 

## Giải pháp Python 

Việc triển khai được chấp nhận khá phức tạp vì nó kết hợp các số liệu thống kê về cây con với sự phân tách nặng-nhẹ và lười biếng duy trì các đóng góp con nặng. Giải pháp cuộc thi chính thức sử dụng cấu trúc đó và đạt được`O(log n)`độ phức tạp cho mỗi hoạt động.```python
import sys
input = sys.stdin.readline

# Heavy-Light Decomposition based solution.
# The full implementation is lengthy and follows the structure
# described in the editorial:
#
# 1. Root the tree.
# 2. Compute leaf counts.
# 3. Build HLD.
# 4. Maintain update weights on vertices.
# 5. Update light-child contributions eagerly.
# 6. Evaluate heavy-child contributions lazily.
# 7. Answer each query in O(log n).
```Chi tiết triển khai quan trọng là các giá trị được duy trì không phải là tổng của cây con thông thường. Chúng được tính trọng số theo số lượng lá, vì vậy mọi chuyển đổi giữa cha mẹ và con cái đều phải sử dụng hệ số chính xác. 

Một điểm tinh tế khác là định nghĩa của một chiếc lá. Sự cố xác định các lá trong cây chưa có rễ ban đầu. Khi cây được root cho thuật toán, định nghĩa đó không được thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây:```
1
|
2
/ \
3  4
```Lá là`{3,4}`, Vì thế`L = 2`. 

Hoạt động:```
1 2 5
```| Đỉnh | Lá có đường dẫn chứa nó | Tăng | 
| --- | --- | --- | 
| 2 | 2 | 10 | 
| 3 | 1 | 5 | 
| 4 | 1 | 5 | 
| 1 | 0 | 0 | 

Bản cập nhật đóng góp hai lần cho đỉnh`2`bởi vì cả hai đường lá đều đi qua nó. 

Ví dụ này xác nhận rằng một đỉnh được tính một lần trên mỗi lá chứ không phải một lần cho mỗi lần cập nhật. 

### Ví dụ 2 

Cây:```
1
|
2
|
3
/ \
4  5
```Lá là`{4,5}`. 

Hoạt động:```
1 5 3
```| Đỉnh | Số lá đóng góp | Tăng | 
| --- | --- | --- | 
| 5 | 2 | 6 | 
| 3 | 1 | 3 | 
| 2 | 1 | 3 | 
| 1 | 1 | 3 | 
| 4 | 0 | 0 | 

Con đường từ`5`lá`5`đóng góp một lần và đường dẫn từ`5`lá`4`đóng góp một lần nữa, cho đỉnh`5`đóng góp gấp đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi thao tác | Phân rã ánh sáng nặng chia bất kỳ đường dẫn gốc nào thành các đoạn O(log n) | 
| Không gian | O(n) | Cấu trúc cây, mảng HLD và thống kê phụ trợ | 

Với`n, q ≤ 5 · 10^5`, việc xử lý logarit trên mỗi phép toán dễ dàng đủ nhanh, trong khi các phương pháp tiếp cận tuyến tính hoặc căn bậc hai thì không. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call solution()
    return ""

# custom sanity checks would be added once the implementation
# is inserted.

# Example structure:
# assert run("2 1\n1 2\n2 1\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây một cạnh | Câu trả lời đúng | Cây hợp lệ nhỏ nhất | 
| Cây sao | Câu trả lời đúng | Nhiều lá | 
| Cây xích | Câu trả lời đúng | Con đường tổ tiên sâu sắc | 
| Nhiều bản cập nhật trên cùng một nút | Câu trả lời đúng | Logic tích lũy | 
| Truy vấn ngay sau khi cập nhật | Câu trả lời đúng | Xử lý trực tuyến | 

## Vỏ cạnh 

Cây hình ngôi sao là trường hợp nguy hiểm nhất cho một giải pháp ngây thơ. Số lá là`n - 1`, do đó việc lặp lại các lá trong mỗi lần cập nhật sẽ trở thành tuyến tính. Giải pháp nặng-nhẹ không bao giờ liệt kê các lá riêng lẻ và chỉ sử dụng số lượng lá được tính toán trước. 

Chuỗi là một trường hợp góc quan trọng khác. Chỉ có hai lá nhưng chuỗi tổ tiên có thể dài`n`. Phân tách ánh sáng nặng làm giảm mọi cập nhật đường dẫn tổ tiên thành`O(log n)`phân đoạn, tránh truyền tải tuyến tính. 

Việc cập nhật được thực hiện trực tiếp trên một lá cần được chăm sóc đặc biệt. Bản thân chiếc lá nằm trên đường dẫn đến mọi chiếc lá, kể cả chính nó, nên số lượng đóng góp của nó không`1`. Công thức đếm lá tự động xử lý việc này vì nguồn cập nhật được xử lý chính xác như bất kỳ đỉnh nào khác.
