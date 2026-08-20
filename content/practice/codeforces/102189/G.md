---
title: "CF 102189G - \u041f\u0438\u0440\u043e\u0433"
description: "Chúng ta có một hình chữ nhật có chiều rộng A và chiều cao B. Một điểm duy nhất bên trong nó được nối với cả bốn góc, tạo ra bốn mảnh hình tam giác."
date: "2026-08-19T06:22:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "G"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 87
verified: true
draft: false
---

[CF 102189G - \u041f\u0438\u0440\u043e\u0433](https://codeforces.com/problemset/problem/102189/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta có một hình chữ nhật có chiều rộng A và chiều cao B. Một điểm duy nhất bên trong nó được nối với cả bốn góc, tạo ra bốn mảnh hình tam giác. Bốn vị khách yêu cầu tỷ lệ phần trăm p 1 ​ ,p 2 ​ ,p 3 ​ ,p 4 ​ trên tổng diện tích và nhiệm vụ là quyết định xem liệu một số vị trí đặt điểm có thể tạo ra chính xác bốn khu vực đó hay không. 

Các vị khách không bị ràng buộc vào các góc cụ thể nên chúng ta có thể tự do ấn định bốn tỷ lệ phần trăm được yêu cầu cho bốn mảnh hình tam giác theo bất kỳ thứ tự nào. Nếu phép gán như vậy tồn tại, chúng ta xuất tọa độ tương ứng X, Y. Vì mọi p i ​ đều dương nên mọi phần được yêu cầu đều có diện tích dương, do đó, một điểm hợp lệ sẽ tự động nằm hoàn toàn bên trong hình chữ nhật. 

Kích thước hình chữ nhật tối đa là 100, trong khi luôn có chính xác bốn phần trăm. Các kích thước nhỏ tạo ra lực lượng mạnh mẽ bằng số, nhưng hình học cho chúng ta một giải pháp theo thời gian không đổi hoàn toàn không phụ thuộc vào kích thước của hình chữ nhật. Chỉ có 4!=24 cách có thể để đặt bốn phần trăm xung quanh hình chữ nhật, vì vậy việc kiểm tra tất cả chúng thực sự là một khoảng thời gian không đổi. 

Trường hợp chính là tỷ lệ phần trăm trông bằng nhau hoặc tương tự nhau không tự động tạo thành cấu hình hợp lệ. Ví dụ,```
1 133 33 33 1
```phải sản xuất`NO`. Một cách tiếp cận bất cẩn có thể cố gắng đặt ba phần 33% cạnh nhau và cho rằng một điểm có thể nhận ra bốn phần dương bất kỳ có tổng bằng 100%. Bốn diện tích được tạo bởi một điểm thỏa mãn mối quan hệ nhân bổ sung, do đó chỉ riêng điều kiện tổng là không đủ. 

Một trường hợp tế nhị khác là các phần bằng nhau:```
3 425 25 25 25
```Đầu ra đúng là`YES`, với tâm (1.5,2) là một câu trả lời hợp lệ. Một giải pháp chỉ xem xét tọa độ nguyên sẽ loại bỏ trường hợp này một cách không chính xác vì tọa độ X được yêu cầu là phân số. 

Trường hợp thứ ba là cấu hình hợp lệ với tất cả tọa độ là phân số:```
10 206 24 56 14
```Câu trả lời đúng là`YES`, với X=2 và Y=6. Chỉ thử điểm giữa hoặc chỉ phần trăm số nguyên của các kích thước sẽ bỏ lỡ cấu trúc chung. 

# Phương pháp tiếp cận 

Phương pháp số trực tiếp có thể liệt kê các tọa độ chuẩn hóa có thể có x=X/A và y=Y/B, tính toán bốn diện tích kết quả và so sánh chúng với tỷ lệ phần trăm được yêu cầu trong tất cả các phép tính. Bởi vì tỷ lệ phần trăm là số nguyên, nên một nghiệm hợp lệ có x và y với mẫu số nhiều nhất là 100, vì vậy việc kiểm tra lưới tọa độ chuẩn hóa 101 x 101 là đủ. Kết hợp điều đó với tất cả 24 bài tập cần tối đa 101 2 ⋅24=244.824 lần kiểm tra, tốc độ này thực sự đủ nhanh cho những ràng buộc này. 

Vấn đề với cách tiếp cận đó không phải là hiệu suất ở đây mà là đối số lưới rất dễ mắc sai lầm. Lưới dấu phẩy động chung không đảm bảo về mặt toán học và việc liệt kê tọa độ với độ chính xác tùy ý nhanh chóng trở nên tốn kém. Ví dụ: một lưới có 10 6 giá trị có thể có cho mỗi tọa độ sẽ yêu cầu khoảng 10 12 cặp tọa độ hoặc khoảng 2,4⋅10 13 so sánh sau khi xem xét tất cả các phép gán. Quan trọng hơn, không có lý do gì để ước chừng một bài toán mà các điều kiện của nó có thể được kiểm tra một cách chính xác. 

Quan sát chính xuất phát từ việc viết bốn diện tích tam giác theo tọa độ chuẩn hóa. Đặt 

x= A X ​ ,y= B Y ​ . 

Giả sử bốn tỷ lệ phần trăm, được biểu thị dưới dạng phân số của toàn bộ hình chữ nhật, được sắp xếp theo chu kỳ thành q 1 ​ ,q 2​ ,q 3 ​ ,q 4 ​, bắt đầu từ tam giác phía dưới bên trái và đi xung quanh hình chữ nhật. Diện tích của chúng chia cho diện tích hình chữ nhật là 

q 1​ =xy, 
q 2 ​ =(1−x)y, 
q 3 ​ =(1−x)(1−y), 
q 4 ​ =x(1−y). 

Từ các phương trình này, 

q 1 ​ q 3 ​ =xy(1−x)(1−y) 

và 

q 2 ​ q 4 ​ =(1−x)yx(1−y), 

nhất thiết phải như vậy 

q 1 ​ q 3 ​ =q 2 ​ q 4 ​ . 

Điều kiện này cũng đủ. Nếu bốn phần trăm dương thỏa mãn đẳng thức đó và tổng bằng 1 thì 

y=q 1 ​ +q 2 ​ 

và 

x=q 1 ​ +q 4 ​ 

sản xuất chính xác bốn lĩnh vực đó. 

Vì khách có thể được chỉ định vào các góc theo bất kỳ thứ tự nào, chúng tôi chỉ cần kiểm tra tất cả 24 hoán vị và tìm kiếm một hoán vị thỏa mãn đẳng thức tích số. Mọi thứ đều có thể được thực hiện với số nguyên, sử dụng p 1 ​ p 3 ​ =p 2 ​ p 4 ​, do đó không có vấn đề về độ chính xác của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới tọa độ | O(101 2 ⋅4!) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Hoán vị + nhận dạng diện tích | O(4!)=O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc A, B và bốn tỷ lệ phần trăm được yêu cầu. 
2. Tạo mọi hoán vị của bốn phần trăm. Một hoán vị thể hiện việc gán tỷ lệ phần trăm cho bốn hình tam giác theo thứ tự tuần hoàn xung quanh điểm đã chọn. 
3. Đối với một hoán vị (q 1 ​ ,q 2​ ,q 3 ​ ,q 4 ​ ), hãy kiểm tra xem 

q 1 ​ q 3 ​ =q 2 ​ q 4 ​ . 

Đây chính xác là đồng nhất thức nhân do bốn tam giác gặp nhau tại một điểm. Bởi vì tất cả các giá trị đều là số nguyên nên việc so sánh là chính xác. 

1. Nếu nhận dạng không thành công, hãy thử hoán vị tiếp theo. Nếu thành công, hãy khôi phục tọa độ chuẩn hóa từ tổng các phần liền kề: 

x=q 1 ​ +q 4 ​ ,y=q 1 ​ +q 2 ​ , 

trong đó tỷ lệ phần trăm được hiểu là phân số, do đó giá trị thực tế là 

x= 100 q 1 ​ +q 4 ​ ​ ,y= 100 q 1 ​ +q 2 ​ ​ . 

1. Chuyển đổi tọa độ chuẩn hóa về hình chữ nhật ban đầu: 

X=A 100 q 1 ​ +q 4 ​ ​ ,Y=B 100 q 1 ​ +q 2 ​ ​ . 

đầu ra`YES`, theo sau là hai tọa độ này. 

1. Nếu tất cả 24 hoán vị đều không nhận dạng được sản phẩm, đầu ra`NO`. Không có vị trí hợp lệ nào có thể tồn tại vì mọi khả năng gán các quân cờ được yêu cầu vào bốn góc đều bị từ chối. 

### Tại sao nó hoạt động 

Đối với mọi vị trí có thể có của điểm, bốn diện tích tam giác chuẩn hóa có dạng xy,(1−x)y,(1−x)(1−y),x(1−y), do đó các tích đối diện luôn bằng nhau. Vì vậy, mọi sự sắp xếp hợp lệ đều phải vượt qua danh tính đã được kiểm tra. Ngược lại, giả sử một hoán vị vượt qua danh tính. Đặt x=(q 1 ​ +q 4 ​ )/100 và y=(q 1 ​ +q 2 ​ )/100. Điều kiện tổng và đẳng thức tích cho q 1 ​ =100xy, q 2 ​ =100(1−x)y, q 3 ​ =100(1−x)(1−y) và q 4 ​ =100x(1−y). Do đó điểm được xây dựng tạo ra chính xác bốn khu vực được yêu cầu. Vì tất cả phần trăm đều dương và có tổng bằng 100 nên cả hai tọa độ đều nằm hoàn toàn giữa các ranh giới của hình chữ nhật. 

#Giải pháp Python```python
Pythonimport sysimport itertools
input = sys.stdin.readline

def solve():    A, B = map(int, input().split())    p = list(map(int, input().split()))
    for q1, q2, q3, q4 in itertools.permutations(p):        if q1 * q3 != q2 * q4:            continue
        X = A * (q1 + q4) / 100.0        Y = B * (q1 + q2) / 100.0
        print("YES")        print(X)        print(Y)        return
    print("NO")

if __name__ == "__main__":    solve()
```Vòng lặp hoán vị tương ứng trực tiếp với việc gán bốn vị khách cho bốn mảnh hình tam giác theo thứ tự tuần hoàn. Chỉ có 24 hoán vị và sự trùng lặp giữa các tỷ lệ phần trăm bằng nhau không gây ra vấn đề về tính chính xác vì việc kiểm tra cùng một sự sắp xếp nhiều lần là vô hại. 

Việc so sánh sản phẩm sử dụng tỷ lệ phần trăm nguyên ban đầu thay vì phân số dấu phẩy động. Nhân với 100 số hủy từ cả hai vế, cho ra chính xác q 1 ​ q 3 ​ =q 2 ​ q 4 ​. Điều này tránh tất cả các vấn đề với các giá trị như 0,33 không thể được biểu diễn chính xác bằng dấu phẩy động nhị phân. 

Khi tìm thấy một hoán vị hợp lệ, q 1 ​ +q 4 ​ là phần của hình chữ nhật nằm bên trái điểm. Do đó nó bằng X/A. Tương tự, q 1 ​ +q 2 ​ là phân số nằm bên dưới điểm nên bằng Y/B. Nhân với A và B sẽ cho ra tọa độ thực. 

Ở đây, đầu ra dấu phẩy động của Python là đủ vì thẩm phán chấp nhận tọa độ với độ chính xác bằng số. Kiểm tra sự tồn tại cơ bản là hoàn toàn chính xác, do đó số học dấu phẩy động chỉ được sử dụng để trình bày tọa độ cuối cùng. 

# Ví dụ đã hoạt động 

## Mẫu 1 

Đầu vào là```
3 425 25 25 25
```Hoán vị đầu tiên đã có sự bằng nhau về tích đối diện cần thiết. 

| Bước | q 1 ​ | q 2 ​ | q 3 ​ | q 4 ​ | q 1 ​ q 3 ​ | q 2 ​ q 4 ​ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Kiểm tra hoán vị | 25 | 25 | 25 | 25 | 625 | 625 | hợp lệ | 
| Tính X | 25 | 25 | 25 | 25 | | | 3⋅50/100=1,5 | 
| Tính Y | 25 | 25 | 25 | 25 | | | 4⋅50/100=2 | 

Điểm là (1.5,2). Mỗi tam giác có diện tích 3⋅4/4=3, chính xác bằng 25% diện tích hình chữ nhật. 

## Mẫu 2 

Đầu vào là```
1 133 33 33 1
```Đối với bất kỳ hoán vị nào, giá trị 1 chiếm một vị trí và ba giá trị 33 chiếm các vị trí khác. Các tích đối diện chỉ có thể là 33⋅33=1089 ở một bên và 33⋅1=33 ở bên kia hoặc có cùng giá trị ở các vị trí ngược lại. 

| Kiểu sắp xếp | q 1 ​ q 3 ​ | q 2 ​ q 4 ​ | Kết quả | 
| --- | --- | --- | --- | 
| 1 đối diện 33 | 33 | 1089 | Không hợp lệ | 
| 33 đối diện 1 | 1089 | 33 | Không hợp lệ | 

Không có hoán vị nào thỏa mãn nhận dạng cần thiết, do đó thuật toán in`NO`. Dấu vết cho thấy tại sao bốn vùng dương có tổng bằng 100% là không đủ để đảm bảo rằng một điểm có thể tạo ra chúng. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(4!)=O(1) | Tối đa 24 hoán vị được kiểm tra, với số học theo thời gian không đổi cho mỗi | 
| Không gian | O(1) | Chỉ có bốn phần trăm và một lượng dữ liệu tạm thời không đổi được lưu trữ | 

Kích thước hình chữ nhật có thể lớn tới 100 nhưng chúng không ảnh hưởng đến số lượng trường hợp được kiểm tra. Thuật toán chỉ thực hiện vài chục phép nhân và cộng số nguyên nên thoải mái trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

# Trường hợp thử nghiệm 

Khai thác kiểm tra bên dưới kiểm tra các thuộc tính cấu trúc của câu trả lời thay vì yêu cầu một tọa độ hợp lệ cụ thể, bởi vì vấn đề cho phép bất kỳ vị trí hợp lệ nào. Nó chạy logic giải pháp tương tự và xác thực tọa độ được tạo bằng cách xây dựng lại tỷ lệ phần trăm của bốn tam giác.```python
Pythonimport sysimport ioimport itertools

def solve_case(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    out = io.StringIO()    sys.stdout = out
    try:        A, B = map(int, sys.stdin.readline().split())        p = list(map(int, sys.stdin.readline().split()))
        for q1, q2, q3, q4 in itertools.permutations(p):            if q1 * q3 != q2 * q4:                continue
            X = A * (q1 + q4) / 100.0            Y = B * (q1 + q2) / 100.0
            print("YES")            print(X)            print(Y)            return out.getvalue()
        print("NO")        return out.getvalue()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

def run(inp: str) -> str:    return solve_case(inp)

def parse_valid_output(inp: str, output: str) -> bool:    lines = output.strip().splitlines()    if lines[0] == "NO":        return False
    assert lines[0] == "YES"    X = float(lines[1])    Y = float(lines[2])
    first, second = inp.strip().splitlines()    A, B = map(float, first.split())    p = list(map(int, second.split()))
    x = X / A    y = Y / B
    areas = [        100.0 * x * y,        100.0 * (1.0 - x) * y,        100.0 * (1.0 - x) * (1.0 - y),        100.0 * x * (1.0 - y),    ]
    remaining = areas[:]    for value in p:        found = False        for i, area in enumerate(remaining):            if abs(area - value) < 1e-7:                remaining.pop(i)                found = True                break        if not found:            return False
    return -1e-9 <= x <= 1.0 + 1e-9 and -1e-9 <= y <= 1.0 + 1e-9

# Provided samplesassert parse_valid_output(    "3 4\n25 25 25 25\n",    run("3 4\n25 25 25 25\n")), "sample 1"
assert run("1 1\n33 33 33 1\n").strip() == "NO", "sample 2"

# Minimum-size rectangle and equal piecesassert parse_valid_output(    "1 1\n25 25 25 25\n",    run("1 1\n25 25 25 25\n")), "minimum rectangle"

# A non-symmetric valid configurationassert parse_valid_output(    "10 20\n6 24 56 14\n",    run("10 20\n6 24 56 14\n")), "valid asymmetric case"

# Maximum-size rectangle with a valid configurationassert parse_valid_output(    "100 100\n6 24 56 14\n",    run("100 100\n6 24 56 14\n")), "maximum rectangle"

# A case close to the invalid sample, catching incorrect sum-only logicassert run("100 1\n40 30 20 10\n").strip() == "NO", "invalid product relation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 25 25 25 25`|`YES`| Kích thước tối thiểu và tọa độ phân đoạn | 
|`10 20 / 6 24 56 14`|`YES`| Cấu hình hợp lệ bất đối xứng chung | 
|`100 100 / 6 24 56 14`|`YES`| Kích thước tối đa | 
|`100 1 / 40 30 20 10`|`NO`| Tình trạng sản phẩm chứ không chỉ đơn thuần là tổng tỷ lệ phần trăm | 

# Vỏ cạnh 

cho```
1 125 25 25 25
```hoán vị đầu tiên trôi qua vì 25⋅25=25⋅25. Các công thức tọa độ cho X=1(25+25)/100=0,5 và Y=1(25+25)/100=0,5. Thuật toán chấp nhận trung tâm, cho thấy tại sao phép liệt kê tọa độ số nguyên sẽ không chính xác. 

Vì```
1 133 33 33 1
```mọi hoán vị có một tích đối diện bằng 33⋅33 và cạnh kia bằng 33⋅1. Vì 1089  =33 nên mọi hoán vị đều bị bác bỏ và kết quả đầu ra là`NO`. Điều này mắc phải sai lầm khi cho rằng có thể thực hiện được các tỷ lệ phần trăm dương tùy ý tổng bằng 100. 

Vì```
10 206 24 56 14
```thứ tự đã cho tự nó hoạt động vì 

6⋅56=336 

và 

24⋅14=336. 

Việc tính toán tọa độ cho 

X=10⋅ 100 6+14 ​ =2 

và 

Y=20⋅ 100 6+24 ​ =6. 

Bốn vùng được chuẩn hóa là 0,06,0,24,0,56,0,14, khớp chính xác với tỷ lệ phần trăm được yêu cầu. 

Vì```
100 140 30 20 10
```bản thân các kích thước không quan trọng đối với bài kiểm tra sự tồn tại. Thuật toán kiểm tra tất cả 24 cách sắp xếp, nhưng không có cách sắp xếp nào thỏa mãn đẳng thức tích đối diện. Ví dụ: thứ tự tự nhiên là 40⋅20=800 và 30⋅10=300. Các hoán vị khác chỉ chọn các cặp đối diện khác nhau và không có hoán vị nào có tích bằng nhau. Kết quả đúng là`NO`, chứng tỏ rằng việc thay đổi kích thước hình chữ nhật không thể cứu được tập hợp bốn phần trăm không thể thực hiện được.
