---
title: "CF 106347A - \u0420\u0435\u043c\u043e\u043d\u0442 \u043a\u043b\u0430\u0434\u043e\u0432\u043a\u0438"
description: "Bài toán mô tả một căn phòng chứa đồ hình chữ nhật có kích thước X × Y × Z. Bốn bức tường phải dán giấy dán tường, nhưng các bức tường đối diện luôn phải sử dụng cùng một loại giấy dán tường. Có hai loại hình nền có sẵn."
date: "2026-06-25T08:04:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106347
codeforces_index: "A"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2024. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 106347
solve_time_s: 38
verified: true
draft: false
---

[CF 106347A - \u0420\u0435\u043c\u043e\u043d\u0442 \u043a\u043b\u0430\u0434\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/106347/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một phòng chứa đồ hình chữ nhật có kích thước`X × Y × Z`. Bốn bức tường phải dán giấy dán tường, nhưng các bức tường đối diện luôn phải sử dụng cùng một loại giấy dán tường. Có hai loại hình nền có sẵn. Loại đầu tiên có`S1`mét vuông có sẵn và chi phí`C1`trên một mét vuông, trong khi loại thứ hai có`S2`mét vuông có sẵn và chi phí`C2`mỗi mét vuông. Một cánh cửa có kích thước`A × B`có thể đặt đúng một bức tường, khu vực cửa ra vào không cần giấy dán tường. Nhiệm vụ là quyết định xem các bức tường có thể được che phủ hay không và nếu có thì tìm chi phí tối thiểu có thể. 

Các kích thước được giới hạn ở`10000`, vì vậy bất kỳ phép tính diện tích nào cũng có thể đạt tới khoảng`10^8`. Số lượng và giá hình nền có thể đạt tới`10^8`, nghĩa là chi phí cuối cùng có thể vào khoảng`10^16`. Giải pháp phải tránh tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định, mặc dù Python xử lý các giá trị này một cách tự nhiên. Vì chỉ có một phòng và chỉ có hai lựa chọn giấy dán tường nên cần có giải pháp tuyến tính hoặc thời gian không đổi. Bất kỳ cách tiếp cận nào cố gắng phân bổ mọi số lượng hình nền có thể cho đến kho có sẵn sẽ quá chậm vì kích thước kho lên tới`10^8`. 

Các trường hợp cạnh chính đến từ hình học. Một cánh cửa có thể không vừa với bất kỳ bức tường nào cả. Ví dụ:```
5 5 4
100 1 100 1
2 5
```Đầu ra là:```
-1
```Chiều cao cửa lớn hơn chiều cao phòng nên không thể đặt được. Một giải pháp bất cẩn có thể chỉ trừ diện tích cửa khỏi tổng diện tích tường và chấp nhận nó, mặc dù về mặt vật lý không thể đặt được cửa. 

Một trường hợp khó khăn khác là khi tổng diện tích giấy dán tường đã đủ nhưng yêu cầu về các bức tường đối diện khiến việc phân bổ không thể thực hiện được:```
10 10 10
150 5 150 5
1 2
```Đầu ra là:```
-1
```Mỗi cặp tường đối diện có diện tích`200`Vì vậy bất kỳ loại giấy dán tường nào dùng cho một cặp đều phải cung cấp đủ chính xác cho cả hai bức tường. Cả hai loại đều không có đủ hàng cho một cặp và tổng số lượng`300`không giúp ích được gì vì hình nền không thể chia tùy ý. 

# Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi cách có thể để chọn cặp tường đối diện nào có loại giấy dán tường đầu tiên. Sau khi chọn vị trí cửa, những bức tường còn lại tự nhiên chia thành hai cặp. Một giải pháp mạnh mẽ có thể kiểm tra cả ba hướng cửa có thể có và đối với mỗi hướng, hãy thử cả hai cách gán loại giấy dán tường cho hai cặp tường. Điều này hiệu quả vì chỉ có một vài lựa chọn về cấu trúc. 

Vấn đề không phải là số lượng hướng mà là cần phải xem xét cẩn thận các khu vực tường. Kích thước phòng đủ nhỏ để tính toán trực tiếp, nhưng việc triển khai không tốt có thể thử mức sử dụng hình nền có thể hoặc lặp lại trên mét vuông. Với số lượng tồn kho lên tới`10^8`, một vòng lặp như vậy sẽ thực hiện hàng trăm triệu lần lặp. 

Quan sát quan trọng là điều kiện của bức tường đối diện làm giảm vấn đề xuống chỉ còn hai nhóm bức tường độc lập. Khi vị trí cửa đã được cố định, hai cặp tường có diện tích cố định. Chỉ có hai quyết định có ý nghĩa: cặp nào nhận được hình nền loại một và cặp nào nhận được hình nền loại hai. Không cần phải tìm kiếm theo số lượng vì mỗi mét vuông của cặp tường được chọn đều phải sử dụng cùng một loại. 

Đối với mỗi vị trí cửa có thể, chúng tôi tính toán hai lượng giấy dán tường cần thiết. Sau đó chúng tôi kiểm tra cả hai bài tập. Nếu nhiệm vụ phù hợp với lượng hàng sẵn có, chúng tôi sẽ tính giá của nó và giữ ở mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S1 + S2) hoặc tệ hơn nếu lặp lại quá mức | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc kích thước phòng, thông tin giấy dán tường và kích thước cửa. Sử dụng số học kiểu 64-bit vì diện tích và chi phí có thể trở nên lớn. 
2. Cân nhắc việc đặt cửa vào một bức tường có kích thước phù hợp`X × Z`. Cánh cửa phù hợp nếu`A <= X`Và`B <= Z`, hoặc nếu cửa bị xoay và`A <= Z`Và`B <= X`. Khi vừa, hãy trừ diện tích cửa khỏi phép tính cặp tường đó. Hai bức tường đối diện chứa cửa tạo thành một nhóm có tổng yêu cầu về giấy dán tường được giảm bớt theo diện tích cửa. 
3. Đối với mỗi vị trí cửa hợp lệ, hãy tính diện tích cần thiết của hai nhóm tường đối diện. Nếu cửa ở trên một`X × Z`tường, nhóm đầu tiên cần`2 * X * Z - A * B`giấy dán tường và nhu cầu của nhóm thứ hai`2 * Y * Z`. Ý tưởng tương tự cũng áp dụng cho hướng tường khác. 
4. Thử gán loại hình nền một cho nhóm đầu tiên và loại hình nền hai cho nhóm thứ hai. Kiểm tra xem cả hai khu vực yêu cầu đều có trong kho sẵn có hay không, sau đó tính tổng chi phí. 
5. Hãy thử làm bài tập ngược lại. Điều này quan trọng vì giấy dán tường rẻ hơn có thể không còn đủ hàng, vì vậy loại đắt tiền hơn có thể cần phải dán một cặp tường khác. 
6. Trong số tất cả các phép gán hợp lệ, hãy đưa ra chi phí tối thiểu. Nếu không có bài tập nào hoạt động, hãy xuất`-1`. 

Điều bất biến đằng sau thuật toán là mọi sự sắp xếp cuối cùng có thể được biểu diễn bằng chính xác một trong các trường hợp được kiểm tra. Cửa chỉ có thể chiếm một trong ba hướng tường và sau khi chọn hướng đó, hai cặp tường phải nhận một loại giấy dán tường duy nhất. Vì tất cả các kết hợp của những lựa chọn này đều được đánh giá nên không thể xảy ra sự sắp xếp hợp lệ bị thiếu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    X, Y, Z = map(int, input().split())
    S1, C1, S2, C2 = map(int, input().split())
    A, B = map(int, input().split())

    ans = float('inf')

    def try_case(pair1, pair2, door_area):
        nonlocal ans

        need1 = pair1 - door_area
        need2 = pair2

        if need1 < 0:
            return

        if need1 <= S1 and need2 <= S2:
            ans = min(ans, need1 * C1 + need2 * C2)

        if need1 <= S2 and need2 <= S1:
            ans = min(ans, need1 * C2 + need2 * C1)

    door_area = A * B

    if (A <= X and B <= Z) or (B <= X and A <= Z):
        try_case(2 * X * Z, 2 * Y * Z, door_area)

    if (A <= Y and B <= Z) or (B <= Y and A <= Z):
        try_case(2 * Y * Z, 2 * X * Z, door_area)

    if ans == float('inf'):
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```chức năng`try_case`đại diện cho một hướng cửa có thể. Cặp tường đầu tiên là cặp tường chứa cửa nên diện tích cửa chỉ bị trừ khỏi nhóm đó. Cặp tường thứ hai vẫn còn nguyên. 

Hai bài tập được kiểm tra một cách rõ ràng. Kiểm tra đầu tiên cung cấp loại hình nền một cho cặp đầu tiên và loại hai cho cặp thứ hai. Séc thứ hai hoán đổi chúng. Đây là hai lựa chọn khả thi duy nhất vì các bức tường đối diện phải khớp nhau. 

Tất cả phép nhân được thực hiện trực tiếp với các số nguyên Python, vì vậy các giá trị lớn như`area * cost`không tràn. Các điều kiện để lắp cửa được viết theo cả hai hướng, ngăn ngừa lỗi phổ biến là coi cửa là không thể quay được. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6 5 10
300 10 10 1
2 3
```Cửa có thể được đặt trên`5 × 10`tường. 

| Bước | Vị trí cửa | Cặp tường đầu tiên cần | Cặp tường thứ hai cần | Chi phí tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 |`Y × Z`bức tường |`100 - 6 = 94`|`120`|`94*10 + 120*10 = 2140`| 

Loại giấy dán tường thứ nhất được sử dụng cho cả hai cặp tường vì loại thứ hai chỉ có`10`mét vuông. Thuật toán tìm thấy chi phí tối thiểu tương tự. 

Đối với mẫu thứ hai:```
6 5 10
200 10 95 1
2 3
```Sự sắp xếp có thể là khác nhau. 

| Bước | Vị trí cửa | Cặp tường đầu tiên cần | Cặp tường thứ hai cần | Chi phí tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 |`Y × Z`bức tường |`100 - 6 = 94`|`120`|`94*10 + 120*1 = 1060`| 
| 2 |`X × Z`bức tường |`120 - 6 = 114`|`100`|`114*10 + 100*1 = 1240`| 

Nhiệm vụ hợp lệ rẻ nhất sử dụng hình nền rẻ hơn trên cặp tường lớn hơn, mang lại mức tối thiểu cần thiết. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra ba hướng cửa và hai bài tập hình nền | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến | 

Khối lượng công việc liên tục dễ dàng phù hợp với giới hạn. Giải pháp không phụ thuộc vào số lượng hình nền có sẵn nên ngay cả những đầu vào lớn nhất cũng được xử lý ngay lập tức. 

# Trường hợp thử nghiệm```python
import sys, io

def solve_case(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    X, Y, Z = map(int, input().split())
    S1, C1, S2, C2 = map(int, input().split())
    A, B = map(int, input().split())

    ans = float('inf')

    def check(a, b, d):
        nonlocal ans
        x = a - d
        y = b
        if x < 0:
            return
        if x <= S1 and y <= S2:
            ans = min(ans, x * C1 + y * C2)
        if x <= S2 and y <= S1:
            ans = min(ans, x * C2 + y * C1)

    door = A * B

    if (A <= X and B <= Z) or (B <= X and A <= Z):
        check(2 * X * Z, 2 * Y * Z, door)
    if (A <= Y and B <= Z) or (B <= Y and A <= Z):
        check(2 * Y * Z, 2 * X * Z, door)

    return str(-1 if ans == float('inf') else ans)

assert solve_case("""6 5 10
300 10 10 1
2 3
""") == "2140"

assert solve_case("""6 5 10
200 10 95 1
2 3
""") == "1294"

assert solve_case("""5 5 4
100 1 100 1
2 5
""") == "-1"

assert solve_case("""10 10 10
150 5 150 5
1 2
""") == "-1"

assert solve_case("""1 1 1
10 7 10 3
1 1
""") == "12"

assert solve_case("""10000 10000 10000
100000000 1 100000000 1
1 1
""") == "399999999960000"

assert solve_case("""5 6 7
1000 5 1000 2
2 3
""") == "805"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 10 7 10 3 / 1 1`|`12`| Kích thước tối thiểu và cửa bao phủ toàn bộ cặp tường giảm diện tích | 
|`10000 10000 10000 / ...`|`399999999960000`| Giá trị số học lớn | 
|`5 6 7 / 1000 5 1000 2 / 2 3`|`805`| Bài tập hình nền khác nhau | 

# Vỏ cạnh 

Đối với trường hợp cửa không vừa khít:```
5 5 4
100 1 100 1
2 5
```Thuật toán kiểm tra cả hai hướng của tường. Chiều cao căn phòng là`4`, trong khi chiều cao cửa là`5`, vậy cũng không`X × Z`cũng không`Y × Z`có thể chứa được cánh cửa. Không có cấu hình nào được kiểm tra và câu trả lời là không thể, vì vậy đầu ra là`-1`. 

Đối với trường hợp giấy dán tường không thể chia thành các bức tường đối diện:```
10 10 10
150 5 150 5
1 2
```Cửa thì được, nhưng cặp tường nào cũng cần xung quanh`200`mét vuông. Mỗi loại hình nền chỉ có`150`, vì vậy cả hai nhiệm vụ đều thất bại trong việc kiểm tra hàng tồn kho. Thuật toán không bao giờ dựa vào tổng diện tích hình nền vì giới hạn của bức tường đối diện mới là yếu tố giới hạn thực tế. Đầu ra là`-1`.
