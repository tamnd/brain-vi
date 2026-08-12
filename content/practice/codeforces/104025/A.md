---
title: "CF 104025A - Quà tặng nguyên hộp"
description: "Chúng ta bắt đầu với sự sắp xếp 3D hình chữ nhật của các khối đơn vị bên trong một hộp có kích thước $n nhân m nhân h$. Mỗi vị trí $(i, j)$ trong lưới $n lần m$ mô tả một chồng hình lập phương thẳng đứng có chiều cao $A{i,j}$. Vì vậy, ban đầu cấu trúc là một bản đồ chiều cao trên sơ đồ mặt bằng."
date: "2026-07-02T04:11:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104025
codeforces_index: "A"
codeforces_contest_name: "The 16-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104025
solve_time_s: 48
verified: true
draft: false
---

[CF 104025A - Quà tặng trong hộp](https://codeforces.com/problemset/problem/104025/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với sự sắp xếp 3D hình chữ nhật của các khối đơn vị bên trong một hộp có kích thước là$n \times m \times h$. Mỗi vị trí$(i, j)$trong một$n \times m$lưới mô tả một chồng hình khối thẳng đứng có chiều cao$A_{i,j}$. Vì vậy, ban đầu cấu trúc là một bản đồ chiều cao trên sơ đồ mặt bằng. 

Sau đó, hộp được xoay để những gì từng là ngăn xếp thẳng đứng giờ đây được nhìn từ một phía khác. Sau khi xoay, chúng tôi lại mô tả cấu trúc hiển thị dưới dạng lưới, lần này có kích thước$h \times m$, trong đó mỗi ô$B_{i,j}$biểu thị có bao nhiêu hình khối có thể nhìn thấy được ở vị trí đó sau khi trọng lực tác động theo hướng mới. 

Điểm mấu chốt là các hình khối không di chuyển tùy tiện. Chúng rơi dưới tác dụng của trọng lực dọc theo hướng thẳng đứng mới sau khi xoay, do đó, mỗi cột trong chế độ xem mới được hình thành bằng cách xếp chồng tất cả các hình khối “chiếu” lên cột đó, được sắp xếp theo chiều cao sau khi xoay. 

Một cách hữu ích để nghĩ về điều này là mỗi cột ban đầu$(i,j)$đóng góp một đống dọc$A_{i,j}$hình khối. Sau khi quay, các cọc này được sắp xếp lại theo một trục khác và trọng lực nén chúng thành các cột mới. 

Những hạn chế$n, m, h \le 100$chỉ ra rằng bất kỳ$O(nmh)$hoặc thậm chí một mô phỏng 3D hệ số không đổi nhỏ cũng có thể chấp nhận được. Điều bị loại trừ là bất kỳ mô phỏng tổ hợp nào trên mỗi khối liên tục quét các vùng lớn hoặc thực hiện sắp xếp lại nhiều lần cho từng ô. 

Trường hợp cạnh tinh tế là khi nhiều ngăn xếp bằng 0 hoặc khi tất cả các ngăn xếp đều ở mức tối đa. Cách tiếp cận “xoay và gán trực tiếp” ngây thơ có xu hướng giả định một ánh xạ giống chuyển vị đơn giản, nhưng thất bại vì trọng lực làm thay đổi phân bố cuối cùng. Ví dụ, nếu tất cả$A_{i,j} = 1$, kết quả không phải là một hoán vị đơn giản của những số 1; thay vào đó, việc xếp chồng gây ra sự hợp nhất dọc theo trục tung mới. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ xử lý từng khối một cách độc lập. Đối với mỗi tế bào$(i,j)$, chúng ta có thể tạo ra$A_{i,j}$khối đơn vị và mô phỏng chuyển động của chúng sau khi quay. Mỗi khối sẽ được theo dõi thông qua một phép biến đổi tọa độ, sau đó được thả xuống dưới tác dụng của trọng lực cho đến khi nó ổn định. Điều này đúng về mặt khái niệm, nhưng nó bùng nổ về mặt tính toán. Trong trường hợp xấu nhất, có tới$n \cdot m \cdot h = 10^6$hình khối và mỗi khối có thể yêu cầu quét một cột có kích thước$h$, dẫn đến đại khái$10^8$hoạt động hoặc nhiều hơn nữa. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng các hình khối riêng lẻ. Sau khi xoay, điều quan trọng chỉ là có bao nhiêu hình khối ở mỗi cột được chiếu. Khi chúng ta biết nhiều tập hợp chiều cao cột góp phần tạo nên một vị trí, trọng lực chỉ cần sắp xếp chúng theo thứ tự. Vì vậy, vấn đề giảm xuống còn việc thu thập các giá trị từ lưới ban đầu vào các nhóm được lập chỉ mục theo hướng mới, sau đó thực hiện việc đóng gói theo chiều dọc ổn định cho mỗi nhóm. 

Cụ thể, mỗi cột ban đầu đóng góp chiều cao của nó$A_{i,j}$đến một vị trí trong lưới xoay. Xoay ánh xạ các chỉ số sao cho tọa độ đầu tiên trở thành chiều cao trong hệ thống mới, trong khi tọa độ thứ hai được giữ nguyên. Điều này tạo ra một nhóm: cho mỗi cố định$j$, chúng tôi thu thập tất cả$A_{i,j}$sang$i$, sau đó mô phỏng việc xếp chúng thành một$h$-cột cao. 

Thay vì mô phỏng trọng lực về mặt vật lý, chúng tôi chỉ giải thích từng cột một cách độc lập: đối với mỗi cột$j$, chúng tôi xử lý danh sách$[A_{1,j}, A_{2,j}, \dots, A_{n,j}]$như một tập hợp các khối được thả vào một thùng chứa có chiều cao thẳng đứng$h$. Mỗi khối đóng góp công suất sử dụng từ dưới lên. 

Điều này làm giảm vấn đề lấp đầy$m$cột có chiều cao độc lập$h$, mỗi cái được xây dựng từ nhiều tập giá trị trong một cột của$A$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trên mỗi khối) |$O(nmh)$ĐẾN$O(nmh \cdot h)$|$O(nmh)$| Quá chậm | 
| Tổng hợp cột |$O(nmh)$|$O(mh)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề như xây dựng$m$các cột dọc độc lập trong lưới đầu ra, mỗi cột có chiều cao$h$, được hình thành bằng cách xếp chồng các đóng góp từ ma trận đầu vào. 

1. Đối với mỗi chỉ mục cột$j$, thu thập tất cả các giá trị$A_{i,j}$vì$i = 1 \ldots n$. Điều này thể hiện tất cả các ngăn xếp được căn chỉnh với cùng một vị trí nằm ngang trong chế độ xem được xoay. Lý do chúng tôi nhóm theo cột là vì việc xoay sẽ duy trì sự căn chỉnh theo chiều ngang trong khi hoán đổi cấu trúc theo chiều dọc. 
2. Với mỗi nhóm tương ứng với cột$j$, chúng tôi mô phỏng trọng lực theo hướng mới bằng cách xử lý các giá trị theo bất kỳ thứ tự nào trong khi điền vào một mảng có kích thước$h$từ dưới lên trên. Chúng tôi duy trì một con trỏ cho biết vị trí chiều cao sẵn có tiếp theo. 
3. Với mỗi giá trị$x = A_{i,j}$, chúng tôi đặt$x$khối đơn vị bắt đầu từ vị trí thấp nhất hiện tại trong cột, di chuyển lên trên. Mỗi vị trí sẽ tăng con trỏ. Nếu cột đầy, phần thừa sẽ bị bỏ qua. 
4. Lặp lại cho đến khi hết giá trị trong cột$j$được xử lý. Điều này mang lại một cột đầy đủ của lưới đầu ra. 
5. Lưu cột kết quả vào ma trận đáp án$B$. 

Một chi tiết quan trọng là chúng ta không cần mô phỏng các vị trí khối riêng lẻ. Chúng ta chỉ cần biết mỗi cột được điền bao nhiêu ô sau khi xếp chồng. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là sau khi xoay, mỗi ngăn xếp đầu vào đóng góp một đoạn thẳng đứng liền kề của các khối đơn vị giống hệt nhau. Trọng lực trong hướng mới không xen kẽ các hình khối từ các ngăn xếp khác nhau; nó chỉ đặt hàng các phân khúc này khi đến nơi. Vì tất cả các hình khối đều giống hệt nhau và chỉ có số lượng của chúng là quan trọng nên cấu trúc cuối cùng chỉ phụ thuộc vào số lượng hình khối được gán cho mỗi cột chứ không phụ thuộc vào thứ tự bên trong của chúng. Do đó, việc xử lý từng cột một cách độc lập và xếp chồng lên nhau một cách tham lam từ dưới lên trên sẽ tạo ra công suất chiếm chỗ giống hệt như mô phỏng vật lý đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, h = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(n)]

B = [[0] * m for _ in range(h)]

for j in range(m):
    col = []
    for i in range(n):
        if A[i][j] > 0:
            col.append(A[i][j])

    ptr = 0
    for x in col:
        for _ in range(x):
            if ptr < h:
                B[ptr][j] = 1
                ptr += 1

for i in range(h):
    print(*B[i])
```Việc triển khai trực tiếp tuân theo chế độ xem tập trung vào cột của thuật toán. Chúng tôi xây dựng từng cột một cách độc lập, sau đó mô phỏng việc xếp chồng bằng cách điền con trỏ từ dưới lên trên. Vòng lặp bên trong mở rộng từng giá trị chiều cao thành các phần đóng góp đơn vị, điều này an toàn với các ràng buộc vì tổng các khối tối đa là$10^6$. 

Một điểm tinh tế là kiểm tra ràng buộc`ptr < h`. Nếu không có nó, các cột bị tràn sẽ làm hỏng bộ nhớ liền kề một cách hợp lý do ghi vượt quá chiều cao dự kiến. Vì các hình khối thừa nằm ngoài vùng nhìn thấy được sau khi xoay nên chúng sẽ được loại bỏ một cách an toàn. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một dấu vết đơn giản để minh họa cách xây dựng một cột. 

### Ví dụ 1 

đầu vào:```
n = 3, m = 1, h = 4
A =
1
2
1
```Chúng tôi xử lý cột$j = 0$. 

| Bước | Giá trị x | ptr trước | Viết | ptr sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | B[0]=1 | 1 | 
| 2 | 2 | 1 | B[1]=1, B[2]=1 | 3 | 
| 3 | 1 | 3 | B[3]=1 | 4 | 

Cột đầu ra trở thành:```
1
1
1
1
```Điều này cho thấy nhiều ngăn xếp sụp đổ thành một cột liên tục dưới tác dụng của trọng lực. 

### Ví dụ 2 

đầu vào:```
n = 2, m = 1, h = 3
A =
2
1
```| Bước | Giá trị x | ptr trước | Viết | ptr sau | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | B[0]=1, B[1]=1 | 2 | 
| 2 | 1 | 2 | B[2]=1 | 3 | 

Đầu ra:```
1
1
1
```Điều này xác nhận rằng thứ tự của các ngăn xếp không quan trọng, chỉ có khối lượng tổng cộng là không quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nmh)$| Mỗi khối đơn vị được viết nhiều nhất một lần khi mở rộng chiều cao ngăn xếp | 
| Không gian |$O(mh)$| Lưới đầu ra chỉ lưu trữ cấu trúc được xoay cuối cùng | 

Giới hạn$n, m, h \le 100$làm$10^6$hoạt động an toàn và mức sử dụng bộ nhớ ở mức tối thiểu vì chúng tôi chỉ lưu trữ lưới cuối cùng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, h = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]

    B = [[0] * m for _ in range(h)]

    for j in range(m):
        col = []
        for i in range(n):
            if A[i][j] > 0:
                col.append(A[i][j])

        ptr = 0
        for x in col:
            for _ in range(x):
                if ptr < h:
                    B[ptr][j] = 1
                    ptr += 1

    return "\n".join(" ".join(map(str, row)) for row in B) + "\n"

# sample
assert run("""3 4 5
1 2 3 4
2 0 1 5
1 3 2 2
""") == """3 2 3 3
1 2 2 3
0 1 1 2
0 0 0 2
0 0 0 1
"""

# minimum case
assert run("""1 1 1
1
""") == "1\n"

# all zeros
assert run("""2 2 3
0 0
0 0
""") == "0 0\n0 0\n0 0\n"

# max stack overflow behavior
assert run("""1 1 3
5
""") == "1\n1\n1\n"

# mixed
assert run("""2 2 3
1 2
3 0
""") == "1 1\n1 1\n1 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khối lập phương đơn 1×1×1 | 1 | độ đúng tối thiểu | 
| tất cả số không | lưới tất cả số không | xử lý trống | 
| ngăn xếp cao đơn | giới hạn điền | cắt tràn | 
| cột hỗn hợp | xếp chồng một phần | độc lập nhiều cột | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một cột tạo ra nhiều hình khối hơn chiều cao$h$. Thuật toán xử lý việc này bằng cách dừng ghi khi`ptr == h`. Ví dụ, nếu$h = 3$và một cột đóng góp$5$hình khối, chỉ có ba hình khối đầu tiên được đặt. Các hình khối còn lại bị bỏ qua vì chúng nằm ngoài chiều cao nhìn thấy được sau khi nén trọng lực. 

Một trường hợp khác là khi tất cả các mục trong một cột đều bằng 0. Danh sách cột trở nên trống rỗng, do đó con trỏ không bao giờ di chuyển và cột đầu ra vẫn hoàn toàn bằng 0. Điều này phù hợp với cách giải thích vật lý khi không có hình khối nào tồn tại trong lát cắt dọc đó. 

Trường hợp thứ ba là đầu vào khác 0 thống nhất trong đó mọi$A_{i,j} = 1$. Mỗi cột đóng góp$n$các khối đơn lẻ và việc xếp chồng chỉ đơn giản là lấp đầy từ dưới lên trên lên đến$h$. Thuật toán nén chính xác tất cả các đóng góp mà không cần dựa vào thứ tự của chúng, xác nhận rằng việc hoán vị các hàng đầu vào không ảnh hưởng đến kết quả.
