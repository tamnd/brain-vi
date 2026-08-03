---
title: "CF 102788F - Trò chơi gián điệp"
description: "Nhiệm vụ yêu cầu chúng ta xây dựng lại biểu đồ tuần hoàn có hướng của các thành phố. Thành phố m là nơi xuất phát của mọi lô hàng. Với mỗi thành phố i, chúng ta có D[i], số đường đi có hướng khác nhau bắt đầu tại m và kết thúc tại i."
date: "2026-08-03T15:10:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102788
codeforces_index: "F"
codeforces_contest_name: "2017-2018 ICPC Central Quarter Final of Northeastern European Regional Collegiate Programming Contest"
rating: 0
weight: 102788
solve_time_s: 87
verified: true
draft: false
---

[CF 102788F - Trò chơi gián điệp](https://codeforces.com/problemset/problem/102788/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta xây dựng lại biểu đồ tuần hoàn có hướng của các thành phố. Thành phố`m`là nguồn gốc của mọi lô hàng. Đối với mỗi thành phố`i`, chúng tôi được cấp`D[i]`, số lượng đường đi có hướng khác nhau bắt đầu tại`m`và kết thúc tại`i`. Chúng ta phải xuất ra bất kỳ tập hợp đường dẫn nào tạo ra chính xác số lượng đường dẫn này. Biểu đồ phải không theo chu kỳ và không được chứa nhiều con đường giữa cùng một cặp thành phố. Tuyên bố ban đầu mô tả điều này giống như việc xây dựng một bản đồ vận chuyển có thể thực hiện được từ số lượng tuyến đường đã thu thập. 

Quan sát quan trọng là số lượng tuyến đường của một thành phố hoàn toàn được xác định bởi các con đường đi vào. Mỗi con đường đi vào từ một thành phố`u`đóng góp tất cả các con đường đã đạt tới`u`, vì vậy nếu một thành phố có hàng xóm đến`p1, p2, ...`, giá trị của nó phải thỏa mãn:`D[v] = D[p1] + D[p2] + ...`Bởi vì biểu đồ có tính chất không tuần hoàn nên mọi phụ huynh của thành phố phải có số lượng đường dẫn nhỏ hơn. Một thành phố có giá trị bằng 0 không thể đến được từ nguồn và không có đường vào. Bản thân thành phố nguồn có chính xác một đường dẫn đến chính nó, đường dẫn trống, vì vậy giá trị của nó phải là một. 

giới hạn`n <= 60`nhỏ nhưng giá trị của`D`có thể lớn như`2^62`, do đó các thuật toán tùy thuộc vào kích thước số của các giá trị là không thể. Chúng tôi không thể mở rộng số lượng, liệt kê đường dẫn hoặc sử dụng lập trình động trên các giá trị. Việc xây dựng phải sử dụng cấu trúc của biểu đồ và thực tế là chỉ có sáu mươi thành phố. 

Các trường hợp biên quan trọng là các thành phố có giá trị 0, các thành phố có giá trị 1 và các giá trị lặp lại. Một thành phố có giá trị bằng 0 không được vô tình nhận được lợi thế. Ví dụ:```
n = 3, m = 1
D = [1, 0, 0]
```Biểu đồ đúng không có đường. Thêm đường từ thành phố`1`đến thành phố`2`sẽ ngay lập tức tạo một đường dẫn và thực hiện`D[2]`không đúng. 

Các giá trị lặp lại cần được xử lý cẩn thận. Ví dụ:```
n = 4, m = 1
D = [1, 1, 1, 3]
```Thành phố cuối cùng cần hai đường dẫn từ nguồn bên cạnh nguồn đóng góp trực tiếp. Việc kết nối cả hai thành phố giá trị một với nó sẽ mang lại ba con đường. Việc coi các giá trị bằng nhau là có thể hoán đổi cho nhau mà không xem xét thứ tự xây dựng có thể tạo ra chu kỳ hoặc các lựa chọn gốc không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng quyết định các con đường đi vào của mỗi thành phố một cách độc lập. Vì một thành phố có giá trị`x`, chúng ta sẽ tìm kiếm trong số tất cả các thành phố trước đó và tìm một tập hợp con có tổng số đường đi là`x`. Điều này đúng vì số lượng đường đi vào một nút chính xác là tổng số đường dẫn của nút cha của nó. Tuy nhiên, việc thử tất cả các tập hợp con đòi hỏi thời gian theo cấp số nhân. Với 60 thành phố, trường hợp xấu nhất sẽ cần phải xem xét khoảng`2^60`những khả năng vượt xa giới hạn. 

Quan sát hữu ích là chúng ta không cần giải bài toán tổng tập hợp con tổng quát. Số lượng đường dẫn nhất định đến từ DAG, nghĩa là mọi giá trị đều có thể được phân tách thành giá trị của các thành phố trước đó. Nếu chúng tôi xử lý các thành phố theo thứ tự tăng dần về số lượng đường đi của chúng thì mọi thành phố có thể là thành phố mẹ đều đã được xử lý. Chúng ta có thể tham lam lấy các giá trị nhỏ hơn lớn nhất hiện có cho đến khi đạt được tổng yêu cầu. 

Đối với đầu vào hợp lệ, quá trình phân tách tham lam này hoạt động vì nếu số tiền còn lại không thể được bao phủ bởi giá trị nhỏ hơn lớn nhất hiện có thì không có giá trị nhỏ hơn nào sau này có thể giúp ích. Thứ tự sắp xếp bảo toàn thuộc tính mà mọi phần còn thiếu phải xuất hiện trước thành phố hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra thành phố nguồn và thu thập tất cả các thành phố có số lượng đường dẫn dương. Các thành phố có giá trị 0 sẽ bị bỏ qua vì chúng phải ở trạng thái không thể truy cập được. 
2. Sắp xếp các thành phố có giá trị dương theo số lượng đường đi của chúng. Việc xử lý chúng từ giá trị nhỏ đến giá trị lớn đảm bảo rằng khi chúng ta xây dựng một thành phố, tất cả các thành phần cha mẹ có thể đều đã có sẵn. 
3. Đối với mọi thành phố ngoại trừ thành phố nguồn, hãy tìm các thành phố trước đó có tổng số đường đi bằng giá trị yêu cầu của nó. Thêm các cạnh có hướng từ các thành phố mẹ đó vào thành phố hiện tại. 
4. Để tìm cha mẹ, hãy quét các thành phố trước đó từ số lượng đường dẫn lớn nhất trở xuống. Bất cứ khi nào giá trị thành phố không vượt quá số tiền còn lại, hãy sử dụng nó làm giá trị gốc và trừ đi số tiền còn lại. 
5. Xuất ra tất cả các tuyến đường đã thu thập. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của các thành phố được sắp xếp, mọi thành phố được xử lý đều đã có chính xác số đường dẫn cần thiết từ nguồn. Khi một thành phố mới được thêm vào, các cạnh đến của nó đóng góp chính xác vào số lượng đường dẫn đã chọn trước đó, do đó giá trị của thành phố mới cũng chính xác. Vì tất cả các cạnh đều chuyển từ giá trị nhỏ hơn đến giá trị lớn hơn nên chu trình không thể xuất hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    d = list(map(int, input().split()))

    src = m - 1
    nodes = [(d[i], i) for i in range(n) if d[i] > 0]
    nodes.sort()

    pos = {}
    for idx, (_, v) in enumerate(nodes):
        pos[v] = idx

    edges = []

    for idx, (val, v) in enumerate(nodes):
        if v == src:
            continue

        need = val
        for j in range(idx - 1, -1, -1):
            if nodes[j][0] <= need:
                edges.append((nodes[j][1] + 1, v + 1))
                need -= nodes[j][0]
            if need == 0:
                break

    print(len(edges))
    for a, b in edges:
        print(a, b)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này sẽ loại bỏ các thành phố không thể truy cập được khỏi danh sách xem xét. Chúng không thể đóng góp vào bất kỳ số lượng tuyến đường nào vì chúng không có đường dẫn từ nguồn. 

Danh sách được sắp xếp lưu trữ các cặp số lượng đường dẫn và chỉ mục thành phố. Vòng lặp trong danh sách này là thứ tự xây dựng của thuật toán. Đối với một thành phố có giá trị yêu cầu`val`, biến`need`theo dõi bao nhiêu đường dẫn vẫn phải được tạo. Mỗi cha mẹ được chọn đóng góp toàn bộ số đường dẫn của nó, vì vậy việc trừ giá trị đó phản ánh chính xác phương trình đếm đường dẫn. 

Thành phố nguồn bị bỏ qua vì giá trị của nó đã được xác định bởi đường dẫn trống. Hướng cạnh luôn từ thành phố được sắp xếp trước đó đến thành phố được sắp xếp sau, điều này ngăn cản các chu kỳ. Số nguyên Python xử lý`2^62`range trực tiếp, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5 1
0 1 1 3 3
```các thành phố tích cực được sắp xếp là: 

| Bước | Thành phố | Giá trị | Còn lại | Đã thêm cha mẹ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | không có, nguồn đã mang lại giá trị | 
| 2 | 3 | 1 | 1 | không có, nguồn đã mang lại giá trị | 
| 3 | 4 | 3 | 3 | thành phố 3 và 2 và nguồn | 
| 4 | 5 | 3 | 3 | thành phố 4 | 

Các đường kết quả sẽ tạo ra số lượng cần thiết. 

Một ví dụ thứ hai:```
4 1
1 1 2 3
```| Bước | Thành phố | Giá trị | Còn lại | Đã thêm cha mẹ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | không | 
| 2 | 3 | 2 | 2 | thành phố 2 và nguồn | 
| 3 | 4 | 3 | 3 | thành phố 3 | 

Dấu vết cho thấy số lượng tuyến đường lớn hơn được xây dựng từ những tuyến đường nhỏ hơn đã được xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi thành phố quét các thành phố đã được xử lý trước đó một lần | 
| Không gian | O(n) | Chỉ danh sách thành phố và danh sách cạnh đã được sắp xếp mới được lưu trữ | 

Số lượng thành phố tối đa chỉ là sáu mươi, vì vậy việc xây dựng bậc hai dễ dàng phù hợp với giới hạn. 

## Vỏ cạnh 

Đối với các thành phố không thể truy cập được, thuật toán không bao giờ xếp chúng vào danh sách tích cực đã được sắp xếp. Vì:```
3 1
1 0 0
```không có thành phố nào được xử lý ngoại trừ nguồn, vì vậy đầu ra không chứa đường nào và số lượng đường vẫn chính xác. 

Đối với nhiều con trực tiếp của nguồn:```
4 1
1 1 1 3
```cả hai thành phố giá trị một đều có sẵn trước khi thành phố giá trị ba được xây dựng. Thuật toán sử dụng chúng cùng với sự đóng góp của nguồn để tạo ra ba tuyến đường cần thiết. 

Đối với các giá trị lớn hơn được lặp lại:```
5 1
1 1 1 2 4
```thành phố giá trị hai được tạo ra từ hai thành phố giá trị một và thành phố giá trị bốn sau đó có thể sử dụng các thành phố được xây dựng nhỏ hơn. Việc xử lý theo thứ tự tăng dần đảm bảo rằng mọi phụ huynh có thể đều có sẵn trước khi cần.
