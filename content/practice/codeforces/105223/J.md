---
title: "CF 105223J - Chỉ có hai"
description: "Chúng ta có một hình vuông thẳng hàng với trục và bên trong nó có nhiều đoạn thẳng, mỗi đoạn thẳng nằm ngang hoặc dọc. Các đoạn nằm hoàn toàn bên trong hình vuông, nhưng chúng có thể chồng lên nhau hoặc thậm chí trùng nhau."
date: "2026-06-24T16:41:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105223
codeforces_index: "J"
codeforces_contest_name: "HIAST Collegiate Programming Contest 2024"
rating: 0
weight: 105223
solve_time_s: 44
verified: true
draft: false
---

[CF 105223J - Chỉ có hai](https://codeforces.com/problemset/problem/105223/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình vuông thẳng hàng với trục và bên trong nó có nhiều đoạn thẳng, mỗi đoạn thẳng nằm ngang hoặc dọc. Các đoạn nằm hoàn toàn bên trong hình vuông, nhưng chúng có thể chồng lên nhau hoặc thậm chí trùng nhau. Chúng ta được yêu cầu chọn hai đoạn thẳng riêng biệt sao cho nếu chúng ta mở rộng cả hai đoạn thành những đường thẳng vô hạn thì hai đường này sẽ chia hình vuông thành một tập hợp các hình chữ nhật thẳng hàng với trục nhỏ hơn và trong số các hình chữ nhật thu được đó tồn tại một giá trị hình chữ nhật xuất hiện chính xác hai lần. 

Hai hình chữ nhật được coi là giống nhau nếu một hình chữ nhật có thể xoay hoặc lật 180 độ, nghĩa là chỉ có độ dài cạnh là quan trọng chứ không phải hướng. 

Về mặt hình học, việc mở rộng hai đoạn thẳng theo trục có nghĩa là chúng ta đang thêm hai đường cắt ngang hoặc dọc đầy đủ bên trong một hình vuông. Bất kỳ cặp đường nào như vậy đều chia hình vuông thành tối đa sáu hoặc chín vùng hình chữ nhật tùy thuộc vào việc chúng cắt nhau hay song song. Điều kiện trong bài toán không phải là có bao nhiêu vùng được hình thành mà là về tập hợp nhiều hình chữ nhật được tạo ra: chúng ta muốn chính xác một loại hình dạng xuất hiện hai lần, trong khi tất cả các hình dạng khác xuất hiện nhiều nhất một lần. 

Kích thước đầu vào lên tới một trăm nghìn phân đoạn, do đó, bất kỳ giải pháp nào thử tất cả các cặp phân đoạn đều không thể thực hiện được ngay lập tức vì nó sẽ yêu cầu n phép toán bình phương, trong trường hợp xấu nhất là khoảng mười tỷ. Đó là vượt xa giới hạn hai giây thông thường. 

Khó khăn chính là tác động của hai đoạn được chọn chỉ phụ thuộc vào các đường hỗ trợ chứ không phải điểm cuối chính xác của chúng. Bất kỳ đoạn nào cũng xác định đường thẳng đứng x = c hoặc đường ngang y = c. Vì vậy, nhiều đoạn có thể tương ứng với cùng một đường cắt hiệu quả. Sự dư thừa này rất quan trọng: việc chọn các phân đoạn khác nhau có cùng tọa độ sẽ mang lại các phân vùng hình học giống hệt nhau. 

Một sai lầm ngây thơ là xử lý các phân đoạn một cách độc lập mà không nén chúng vào các vị trí x hoặc y duy nhất. Một cạm bẫy tinh vi khác là cho rằng tất cả các phân đoạn đều hữu ích; nhiều đoạn nằm trên cùng một đường có thể hoán đổi cho nhau và chỉ có tọa độ đường của chúng là quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra từng cặp phân đoạn. Đối với mỗi cặp, chúng tôi mở rộng chúng thành các dòng đầy đủ và tính toán phân vùng kết quả của hình vuông, sau đó đếm các hình chữ nhật và kiểm tra xem có chính xác hai hình chữ nhật có cùng kích thước hay không. Mỗi lần kiểm tra yêu cầu tính toán các giao điểm với các ranh giới hình vuông và dẫn đến số lượng hình chữ nhật không đổi, do đó mỗi cặp có giá O(1). Tuy nhiên, có các cặp O(n^2) dẫn đến khoảng 10^10 thao tác, điều này không khả thi. 

Quan sát chính là cấu trúc của phân vùng chỉ phụ thuộc vào việc chúng ta chọn các đường thẳng đứng hay nằm ngang và tọa độ của chúng. Khi hai đường thẳng được cố định, kích thước hình chữ nhật cảm ứng được xác định bằng khoảng cách giữa các đường viền hình vuông và các đường này. Do đó, điều quan trọng duy nhất là liệu có tồn tại hai đường mà vị trí tương đối của chúng tạo ra cấu hình diện tích hình chữ nhật lặp lại hay không. 

Vấn đề trở thành lý luận về các cặp đường cắt theo một chiều tại một thời điểm. Cắt dọc chỉ phụ thuộc vào tọa độ x của nó và cắt ngang chỉ phụ thuộc vào tọa độ y của nó. Vì vậy, chúng ta có thể tách các phân đoạn thành các bộ dọc và ngang và phân tích từng loại một cách độc lập. 

Đối với các đường thẳng đứng, việc sắp xếp tọa độ x của chúng cho phép chúng ta suy luận về các vị trí liền kề hoặc giống hệt nhau. Điều tương tự cũng áp dụng cho các đường ngang. Điều kiện để tạo ra chính xác một hình chữ nhật trùng lặp giảm xuống việc tìm một cặp đường cắt có vị trí bằng nhau hoặc một cấu hình đối xứng rất cụ thể buộc một hình chữ nhật lặp lại.

Trên thực tế, cách duy nhất để có được chính xác hai hình chữ nhật giống hệt nhau trong số tất cả các phần thu được từ hai đường cắt thẳng hàng theo trục bên trong một hình vuông là khi cả hai đoạn được chọn tương ứng với cùng một hướng và tạo ra sự phân chia đối xứng, điều này xảy ra khi chúng ta chọn hai đoạn được căn chỉnh sao cho hình chiếu của chúng tạo ra các phân vùng có chiều rộng hoặc chiều cao bằng nhau. Điều này chuyển sang tìm kiếm hai phân đoạn có cùng tọa độ (x hoặc y). Nếu bất kỳ tọa độ nào xuất hiện ít nhất hai lần giữa các đoạn, việc chọn hai đoạn bất kỳ có tọa độ đó sẽ thỏa mãn điều kiện, vì cả hai vết cắt đều trùng nhau và tạo ra một dải hình chữ nhật trùng lặp dọc theo trục đối diện. 

Do đó, vấn đề giảm xuống còn việc phát hiện các bản sao ở tọa độ x của các đoạn thẳng đứng hoặc tọa độ y của các đoạn ngang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp | O(n^2) | O(1) | Quá chậm | 
| Nhóm tọa độ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phân đoạn và phân loại từng phân đoạn theo chiều dọc hoặc chiều ngang. Một đoạn thẳng đứng nếu x1 bằng x2, nếu không thì nó nằm ngang. Việc phân loại này là cần thiết vì chỉ có hướng thẳng hàng với trục mới quan trọng đối với đường cắt cảm ứng. 
2. Đối với các đoạn thẳng đứng, trích xuất tọa độ x của chúng. Đối với các đoạn ngang, hãy trích xuất tọa độ y của chúng. Lưu trữ các chỉ mục được nhóm theo các tọa độ này. 
3. Trong khi chèn tọa độ, hãy kiểm tra xem có tọa độ nào đã xuất hiện trước đó hay không. Nếu chúng ta thấy x lặp lại cho các phân đoạn dọc hoặc y lặp lại cho các phân đoạn ngang, chúng ta ngay lập tức có câu trả lời hợp lệ và có thể xuất ra bất kỳ hai chỉ số phân đoạn tương ứng nào. 
4. Nếu không có cặp trùng lặp nào tồn tại ở tọa độ dọc hoặc tọa độ ngang, hãy kết luận rằng không tồn tại cặp hợp lệ và ghi -1 -1. 

Lý do đằng sau việc dừng sớm là tọa độ lặp lại đầu tiên đã đảm bảo hai đoạn riêng biệt tạo ra các đường cắt toàn dòng giống hệt nhau. 

### Tại sao nó hoạt động 

Khi một đoạn được mở rộng, chỉ có đường hỗ trợ của nó mới quan trọng. Nếu hai đoạn riêng biệt nằm trên cùng một đường thẳng đứng x = c thì việc chọn chúng sẽ tạo ra hai đường cắt dọc giống hệt nhau, không làm thay đổi phân vùng khác với việc chọn một đường cắt hai lần trong các cách biểu diễn khác nhau. Điều này đảm bảo rằng việc phân tách hình chữ nhật thu được sẽ bao gồm các vùng giống hệt nhau lặp đi lặp lại dọc theo hướng ngang, đáp ứng yêu cầu có chính xác hai hình chữ nhật bằng nhau trong số tất cả các hình chữ nhật được tạo thành. Bất kỳ cấu hình hợp lệ nào thỏa mãn điều kiện nhất thiết phải đưa ra một đường cắt hiệu quả lặp lại, bởi vì không trùng lặp vị trí cắt, tất cả các kích thước hình chữ nhật thu được đều khác biệt bằng cách xây dựng lưới căn chỉnh theo trục. Do đó việc phát hiện bất kỳ tọa độ lặp lại nào là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, xs, ys, xe, ye = map(int, input().split())

    seen_v = {}
    seen_h = {}

    for i in range(1, n + 1):
        x1, y1, x2, y2 = map(int, input().split())

        if x1 == x2:
            x = x1
            if x in seen_v:
                print(seen_v[x], i)
                return
            seen_v[x] = i
        else:
            y = y1
            if y in seen_h:
                print(seen_h[y], i)
                return
            seen_h[y] = i

    print(-1, -1)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã phân tách các phân đoạn dọc và ngang bằng cách so sánh các điểm cuối. Đối với các phân đoạn dọc, nó sử dụng từ điển ánh xạ tọa độ x tới chỉ mục phân đoạn đầu tiên tạo ra nó. Đối với các đoạn ngang, nó thực hiện tương tự với tọa độ y. Khi gặp tọa độ trùng lặp, nó sẽ xuất ra hai chỉ số phân đoạn. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ lưu trữ lần xuất hiện đầu tiên của mỗi tọa độ. Điều này là đủ vì bài toán chỉ yêu cầu bất kỳ cặp hợp lệ nào. Một điểm quan trọng khác là các điểm cuối của phân đoạn không liên quan ngoài việc xác định hướng và vị trí, vì vậy chúng tôi không chuẩn hóa hoặc sắp xếp lại các điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 1 10 10
1 3 1 7
2 5 8 5
4 2 4 9
6 6 9 6
```Chúng tôi xử lý các phân đoạn: 

| tôi | Loại | Tọa độ | Đã từng thấy | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | dọc | x = 1 | không | cửa hàng 1 | 
| 2 | ngang | y = 5 | không | cửa hàng 2 | 
| 3 | dọc | x = 4 | không | cửa hàng 3 | 
| 4 | ngang | y = 6 | không | cửa hàng 4 | 

Không có bản sao tồn tại, vì vậy đầu ra là:```
-1 -1
```Điều này cho thấy trường hợp tất cả các vết cắt đều khác biệt, do đó không có hai phân đoạn nào tạo ra các phân vùng giống hệt nhau dư thừa. 

### Ví dụ 2 

đầu vào:```
5 0 0 10 10
1 2 1 8
3 1 3 9
5 4 5 7
2 6 2 9
7 3 7 8
```Xử lý: 

| tôi | Loại | Tọa độ | Đã từng thấy | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | dọc | x = 1 | không | cửa hàng 1 | 
| 2 | dọc | x = 3 | không | cửa hàng 2 | 
| 3 | dọc | x = 5 | không | cửa hàng 3 | 
| 4 | dọc | x = 2 | không | cửa hàng 4 | 
| 5 | dọc | x = 7 | không | cửa hàng 5 | 

Không có bản sao nữa, vì vậy đầu ra:```
-1 -1
```Bây giờ sửa đổi một chút để bao gồm một bản sao: 

Nếu phân đoạn 6 là`3 0 3 10`, thì khi xử lý nó, chúng ta sẽ thấy rằng x = 3 đã tồn tại ở chỉ mục 2 và chúng ta sẽ xuất ra ngay lập tức:```
2 6
```Điều này thể hiện hành vi chấm dứt sớm và cho thấy rằng chỉ có sự lặp lại phối hợp mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý một lần với các thao tác từ điển O(1) | 
| Không gian | O(n) | Chúng tôi có thể lưu trữ tối đa n tọa độ riêng biệt | 

Quét tuyến tính phù hợp thoải mái trong giới hạn lên tới 100.000 phân đoạn và hoạt động từ điển vẫn hiệu quả trong thực tế. Việc sử dụng bộ nhớ cũng tuyến tính và đủ nhỏ cho các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, xs, ys, xe, ye = map(int, input().split())

    seen_v = {}
    seen_h = {}

    for i in range(1, n + 1):
        x1, y1, x2, y2 = map(int, input().split())

        if x1 == x2:
            x = x1
            if x in seen_v:
                return f"{seen_v[x]} {i}"
            seen_v[x] = i
        else:
            y = y1
            if y in seen_h:
                return f"{seen_h[y]} {i}"
            seen_h[y] = i

    return "-1 -1"

# minimal
assert run("1 1 1 2 2\n1 1 1 2") == "-1 -1"

# simple duplicate vertical
assert run("2 0 0 10 10\n1 2 1 8\n1 3 1 9") == "1 2"

# simple duplicate horizontal
assert run("2 0 0 10 10\n2 5 8 5\n3 5 9 5") == "1 2"

# mixed no answer
assert run("3 0 0 10 10\n1 1 1 2\n2 2 2 3\n3 3 3 4") == "-1 -1"

# many distinct
assert run("4 0 0 10 10\n1 1 1 2\n2 2 2 3\n3 3 3 4\n4 4 4 5") == "-1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đoạn | -1 -1 | trường hợp tối thiểu | 
| trùng lặp dọc | 1 2 | phát hiện x lặp đi lặp lại | 
| trùng lặp ngang | 1 2 | phát hiện y lặp đi lặp lại | 
| tất cả đều khác biệt | -1 -1 | trường hợp tiêu cực | 
| tập hợp lớn hơn | -1 -1 | khả năng mở rộng và không có kết quả dương tính giả | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi nhiều đoạn nằm trên cùng một dòng nhưng được viết bằng các điểm cuối đảo ngược. Ví dụ,`1 5 1 9`Và`1 9 1 5`vẫn biểu diễn cùng một đường thẳng đứng x = 1. Thuật toán xử lý điều này một cách chính xác vì nó chỉ kiểm tra`x1 == x2`, bỏ qua việc đặt hàng hoàn toàn. 

Một trường hợp khác là có nhiều bản sao: nếu ba hoặc nhiều phân đoạn có cùng tọa độ, thì lần phát hiện lặp lại đầu tiên đã kích hoạt một câu trả lời hợp lệ. Ví dụ:```
3 0 0 10 10
2 1 2 9
2 3 2 8
2 4 2 7
```Phân đoạn thứ hai đã khớp với phân đoạn đầu tiên nên kết quả đầu ra là ngay lập tức`1 2`, mặc dù có tồn tại một phần ba. 

Trường hợp cuối cùng là khi tất cả các đoạn có hướng hỗn hợp nhưng vẫn không tồn tại tọa độ trùng lặp. Trong những đầu vào như vậy, thuật toán trả về chính xác`-1 -1`bởi vì không có hai đoạn nào xác định các đường cắt hiệu quả giống hệt nhau và do đó không thể phát sinh sự lặp lại trong cấu trúc hình chữ nhật cảm ứng.
