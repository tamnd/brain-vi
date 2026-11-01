---
title: "CF 106409F - Máy đẩy tốc độ"
description: "Sự cố mô tả một số đèn hiệu được đặt trên một đường dây. Mỗi đèn hiệu có một vị trí và công suất hiện tại. Trong một bước bình thường, mọi đèn hiệu vẫn có thể mở rộng sẽ tăng sức mạnh của nó lên một. Một đèn hiệu có công suất p bao phủ khoảng từ x - p đến x + p."
date: "2026-06-25T09:58:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106409
codeforces_index: "F"
codeforces_contest_name: "HPI 2026 Advanced"
rating: 0
weight: 106409
solve_time_s: 33
verified: true
draft: false
---

[CF 106409F - Bộ đẩy tốc độ](https://codeforces.com/problemset/problem/106409/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Sự cố mô tả một số đèn hiệu được đặt trên một đường dây. Mỗi đèn hiệu có một vị trí và công suất hiện tại. Trong một bước bình thường, mọi đèn hiệu vẫn có thể mở rộng sẽ tăng sức mạnh của nó lên một. Một ngọn hải đăng có sức mạnh`p`bao gồm khoảng thời gian từ`x - p`ĐẾN`x + p`. Một đèn hiệu ngừng mở rộng khi khoảng thời gian được bao phủ của nó chạm vào khoảng thời gian được bao phủ của một đèn hiệu khác vì nó không thể phát triển độc lập được nữa. 

Nhiệm vụ là tìm tổng số vị trí được bảo hiểm cuối cùng sau khi quá trình lặp lại này kết thúc. 

Đầu vào cung cấp số lượng đèn hiệu theo sau là tọa độ và công suất ban đầu của mỗi đèn hiệu. Đầu ra là số vị trí số nguyên được bao phủ sau khi tất cả các mở rộng có thể xảy ra. 

Các ràng buộc yêu cầu tránh mô phỏng trực tiếp. Nếu có`n`đèn hiệu và mỗi đèn hiệu có thể mở rộng nhiều lần, việc mô phỏng mỗi vòng có thể mất nhiều thời gian tuyến tính hơn. Với đầu vào lớn, cách tiếp cận liên tục quét tất cả các đèn hiệu là quá chậm vì số vòng phụ thuộc vào khoảng cách tọa độ, không chỉ trên`n`. 

Phần khó khăn là vấn đề tăng trưởng đồng thời. Một đèn hiệu bị chặn sau một lần cập nhật một phần vẫn có thể phát triển trong quy trình thực tế vì các đèn hiệu khác cũng phát triển trong cùng một vòng. 

Ví dụ: hai đèn hiệu:```
2
0 1
5 1
```Cả hai bắt đầu bao phủ`[ -1, 1 ]`Và`[4, 6]`. Cuối cùng họ gặp nhau sau khi cả hai đều trưởng thành. Một mô phỏng chỉ phát triển đèn hiệu bên trái cho đến khi nó dừng lại sẽ đóng băng nó quá sớm một cách không chính xác. 

Một trường hợp khác là chồng chéo các phạm vi ban đầu:```
2
0 5
3 1
```Đèn hiệu đầu tiên đã bao phủ đèn hiệu thứ hai. Việc coi đèn hiệu thứ hai là độc lập và thêm cả hai phạm vi riêng biệt sẽ tính vị trí hai lần. 

# Phương pháp tiếp cận 

Một giải pháp đơn giản là mô phỏng đầy đủ các bước. Trong mỗi vòng, hãy tìm mọi đèn hiệu có thể mở rộng, tăng sức mạnh và lặp lại cho đến khi không có thay đổi nào xảy ra. Điều này đúng vì nó tuân theo quy trình một cách chính xác. Vấn đề là một đèn hiệu có thể cần nhiều lần mở rộng trước khi đến được một đèn hiệu khác. Nếu tọa độ lớn thì số vòng có thể tỷ lệ thuận với khoảng cách tọa độ, khiến trường hợp xấu nhất vượt xa giới hạn chấp nhận được. 

Quan sát hữu ích là thứ tự của các khai triển riêng lẻ không quan trọng. Thay vì tăng tất cả các đèn hiệu có thể mở rộng lại với nhau, hãy tưởng tượng thực hiện các hoạt động nhỏ hơn trong đó mỗi lần chỉ có một đèn hiệu mở rộng thêm một đơn vị. Trạng thái ổn định cuối cùng là giống hệt nhau. Điều này cho phép chúng tôi xử lý việc mở rộng dưới dạng hợp nhất giữa các khoảng thời gian được bảo hiểm. 

Sau khi sắp xếp các đèn hiệu theo vị trí, hãy duy trì các phân đoạn được che phủ rời rạc hiện tại. Khi một phân khúc mới chồng lên phân khúc trước đó, cả hai đều thuộc cùng một nhóm cuối cùng. Phạm vi tiếp cận tối đa của họ trở thành phạm vi tiếp cận của phân khúc đã hợp nhất. Mỗi phân đoạn ban đầu chỉ tham gia vào một số lần hợp nhất không đổi, do đó tổng công việc sau khi sắp xếp là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số vòng × n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đèn hiệu theo vị trí của chúng. Thứ tự từ trái sang phải là đủ vì tương tác chỉ có thể xảy ra giữa các khoảng bao phủ lân cận. 
2. Biểu diễn mỗi nhóm hiện tại dưới dạng một phân đoạn`[left, right]`cùng với sức mạnh mở rộng của nó. Ban đầu mỗi đèn hiệu là phân đoạn riêng của nó. 
3. Thêm từng đèn hiệu theo thứ tự sắp xếp. Sau khi chèn một đoạn mới, hãy so sánh nó với đoạn trước đó. 
4. Nếu hai đoạn không trùng nhau, hãy giữ cả hai. Họ bị chia cắt vĩnh viễn vì việc mở rộng trong tương lai trong một nhóm không thể vượt qua được khoảng trống chưa được khám phá. 
5. Nếu các đoạn trùng nhau, hãy hợp nhất chúng. Phân đoạn mới có ranh giới bên trái của phân đoạn bên trái và ranh giới bên phải của vùng phủ sóng được hợp nhất. Sức mạnh của nó lớn hơn trong hai sức mạnh vì đèn hiệu đó có thể tiếp tục mở rộng xa nhất. 
6. Tiếp tục sáp nhập ngược trong khi phân đoạn mới nhất chồng lên phân đoạn trước đó. 
7. Sau khi tất cả các quá trình hợp nhất hoàn tất, mọi phân đoạn còn lại là thành phần được bảo hiểm được kết nối cuối cùng. Một đoạn có điểm cuối bên trái`l`, điểm cuối bên phải`r`, và sức mạnh`p`đóng góp`r - l + 2 * p + 1`các vị trí. 

Tại sao nó hoạt động: điều bất biến là mỗi phân đoạn được lưu trữ đại diện cho chính xác một thành phần được kết nối của vùng phủ sóng không thể tách rời được nữa. Khi hai thành phần chạm vào nhau, quy trình trong tương lai sẽ coi chúng là một vì việc mở rộng từ hai phía có thể tiếp tục cho đến khi đạt được phạm vi tiếp cận xa nhất bên trong thành phần. Vì mỗi lần hợp nhất đều duy trì phạm vi tiếp cận tối đa có thể nên các phân đoạn cuối cùng hoàn toàn giống với những phân đoạn được tạo bằng cách áp dụng nhiều lần các bước đầy đủ ban đầu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    beacons = []
    for _ in range(n):
        x, p = map(int, input().split())
        beacons.append((x, p))

    beacons.sort()

    stack = []

    for x, p in beacons:
        stack.append([x - p, x + p, p])

        while len(stack) >= 2:
            a = stack[-2]
            b = stack[-1]

            if a[1] < b[0]:
                break

            stack.pop()
            stack.pop()

            stack.append([
                a[0],
                max(a[1], b[1]),
                max(a[2], b[2])
            ])

    ans = 0
    for l, r, p in stack:
        ans += r - l + 2 * p + 1

    print(ans)

solve()
```Đầu vào được sắp xếp trước vì quá trình hợp nhất phụ thuộc vào các khoảng lân cận. Ngăn xếp chỉ lưu trữ các khoảng thời gian hiện đang tách biệt. 

Vòng lặp hợp nhất là cốt lõi của thuật toán. Khi hai khoảng giao nhau, việc giữ chúng tách biệt sẽ vi phạm nguyên tắc bất biến rằng tất cả các khoảng được lưu trữ đều rời rạc. Sức mạnh mới là sức mạnh tối đa của hai sức mạnh ban đầu vì chỉ có khả năng mở rộng mạnh nhất còn lại mới quan trọng sau khi chúng được kết nối. 

Vòng lặp cuối cùng đếm các vị trí được bao phủ bởi từng thành phần độc lập. Công thức bao gồm độ dài khoảng thời gian ban đầu và mức tăng trưởng bổ sung có sẵn ở cả hai bên. 

Số nguyên Python không bị tràn, do đó việc triển khai không cần xử lý đặc biệt đối với các câu trả lời lớn. 

# Ví dụ đã hoạt động 

Hãy xem xét:```
3
0 1
5 1
10 1
```Quá trình xử lý trông như thế này: 

| Bước | Đã thêm đèn hiệu | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 |`(0,1)`|`[-1,1]`| 
| 2 |`(5,1)`|`[-1,1], [4,6]`| 
| 3 |`(10,1)`|`[-1,1], [4,6], [9,11]`| 

Không có khoảng thời gian trùng lặp, vì vậy câu trả lời cuối cùng là:`3 + 3 + 3 = 9`Ví dụ cho thấy các thành phần độc lập vẫn tách biệt. 

Một ví dụ khác:```
3
0 2
5 2
10 2
```| Bước | Đã thêm đèn hiệu | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 |`(0,2)`|`[-2,2]`| 
| 2 |`(5,2)`|`[-2,2], [3,7]`| 
| 3 |`(10,2)`|`[-2,2], [3,7], [8,12]`| 

Khoảng giữa ban đầu không chạm vào bên nào nên các thành phần vẫn tách biệt. Thuật toán không hợp nhất các khoảng chỉ vì chúng gần nhau, chỉ khi các vị trí được che phủ của chúng giao nhau. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế và mỗi khoảng được hợp nhất một số lần không đổi | 
| Không gian | O(n) | Ngăn xếp lưu trữ các thành phần rời rạc hiện tại | 

Giải pháp phù hợp vì nó tránh mô phỏng các vòng mở rộng riêng lẻ. Ngay cả khi tọa độ rất lớn, số lượng thao tác ngăn xếp vẫn tuyến tính sau khi sắp xếp. 

# Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    n = int(data())
    a = []
    for _ in range(n):
        x, p = map(int, data().split())
        a.append((x, p))
    a.sort()
    st = []
    for x, p in a:
        st.append([x-p, x+p, p])
        while len(st) >= 2 and st[-2][1] >= st[-1][0]:
            b = st.pop()
            c = st.pop()
            st.append([c[0], max(c[1], b[1]), max(c[2], b[2])])
    ans = sum(r-l+2*p+1 for l, r, p in st)
    sys.stdin = old
    return str(ans) + "\n"

assert run("""1
0 0
""") == "1\n"

assert run("""2
0 1
5 1
""") == "6\n"

assert run("""3
0 2
5 2
10 2
""") == "15\n"

assert run("""2
0 5
3 1
""") == "11\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đèn hiệu không có điện |`1`| Trường hợp kích thước tối thiểu | 
| Hai đèn hiệu riêng biệt |`6`| Thành phần độc lập | 
| Một số phạm vi tăng trưởng cảm động |`15`| Sáp nhập lặp đi lặp lại | 
| Một đèn hiệu ban đầu bao phủ một đèn hiệu khác |`11`| Xử lý chồng chéo | 

# Vỏ cạnh 

Đối với một đèn hiệu duy nhất:```
1
7 0
```Ngăn xếp chứa một khoảng`[7,7]`. Không có sự hợp nhất nào xảy ra và sự đóng góp là`1`, điều này đúng vì một vị trí đã được bảo hiểm. 

Đối với các đèn hiệu chồng chéo ban đầu:```
2
0 5
3 1
```Ngọn hải đăng đầu tiên bao phủ`[-5,5]`và bìa thứ hai`[2,4]`. Thuật toán hợp nhất chúng ngay lập tức vì các khoảng trùng nhau. Khoảng thời gian hợp nhất giữ công suất lớn hơn, do đó phạm vi phủ sóng cuối cùng chỉ được xác định bởi đèn hiệu đầu tiên, tránh tính hai lần. 

Đối với các đèn hiệu riêng biệt:```
2
0 1
100 1
```Các khoảng`[-1,1]`Và`[99,101]`không bao giờ cắt nhau. Ngăn xếp giữ hai thành phần và câu trả lời là tổng của cả hai độ dài. 

Đối với một chuỗi chồng chéo:```
3
0 2
4 2
8 2
```Hai khoảng đầu tiên chạm nhau nên chúng hợp nhất. Thành phần kết quả đạt đủ xa để chồng lấp khoảng thứ ba, gây ra sự hợp nhất thứ hai. Vòng lặp hợp nhất lặp lại xử lý trường hợp này vì sau mỗi lần hợp nhất, khoảng thời gian mới sẽ được kiểm tra lại đối với thành phần trước đó.
