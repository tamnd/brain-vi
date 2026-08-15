---
title: "CF 102423I - Kết nối mê cung"
description: "Đầu vào mô tả một mê cung có các bức tường được vẽ theo đường chéo. Mỗi vị trí ký tự là một hình vuông nhỏ trong biểu diễn ASCII. Dấu chấm có nghĩa là hình vuông đó không có bức tường nào. Dấu gạch chéo hoặc dấu gạch chéo ngược là đoạn tường chéo bên trong hình vuông đó."
date: "2026-08-12T04:45:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "I"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 187
verified: true
draft: false
---

[CF 102423I - Kết nối mê cung](https://codeforces.com/problemset/problem/102423/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một mê cung có các bức tường được vẽ theo đường chéo. Mỗi vị trí ký tự là một hình vuông nhỏ trong biểu diễn ASCII. Dấu chấm có nghĩa là hình vuông đó không có bức tường nào. Dấu gạch chéo hoặc dấu gạch chéo ngược là đoạn tường chéo bên trong hình vuông đó. Điều kiện chẵn lẻ trên các hướng gạch chéo đảm bảo rằng các bức tường chéo lân cận gặp nhau một cách nhất quán và không tạo ra các giao cắt không rõ ràng. Nhiệm vụ là loại bỏ càng ít đoạn tường càng tốt để mọi khu vực của mê cung đều có đường dẫn ra bên ngoài. 

Cách hữu ích để nghĩ về mê cung không phải là về các ký tự ban đầu mà là về các vùng không gian trống được kết nối. Giả sử mê cung hiện tại có (k) các vùng bị ngắt kết nối, trong đó bên ngoài là một trong số đó. Việc loại bỏ một bức tường có thể kết nối nhiều nhất hai vùng, do đó cần ít nhất (k-1) bức tường. Ngược lại, mọi bức tường ngăn cách hai vùng khác nhau có thể được loại bỏ để hợp nhất chúng, do đó, việc loại bỏ (k-1) là đủ. Do đó, toàn bộ vấn đề được rút gọn thành việc đếm các vùng được kết nối. 

Sự cố chính thức có (1 \le r,c \le 1000), vì vậy dữ liệu đầu vào có thể chứa tối đa (10^6) ký tự. Một giải pháp kiểm tra lượng công việc không đổi trên mỗi ký tự đầu vào là phù hợp. Một thuật toán có công việc (O((rc)^2)) sẽ đạt được khoảng (10^{12}) hoạt động ở kích thước tối đa và hoàn toàn không phù hợp. Tuyên bố cuộc thi ban đầu sử dụng giới hạn thời gian là 5 giây và bộ nhớ 512 MB. 

Có một số chi tiết hình học mà việc triển khai lưới đơn giản có thể mắc sai lầm. Một bức tường chéo không thể đơn giản được coi là một ô ký tự bị chặn, bởi vì hai cạnh của đường chéo đó thuộc về các vùng khác nhau. Ví dụ, mê cung khép kín nhỏ nhất là```
2 2
/\
\/
```và câu trả lời là`1`. Việc coi mỗi dấu gạch chéo như một ô vuông lưới bị chặn thông thường sẽ làm mất hình dạng đường chéo và có thể báo cáo không chính xác rằng không có vùng kèm theo. 

Trường hợp cạnh thứ hai là một mê cung có các bức tường chéo chạm vào ranh giới bên ngoài nhưng không tạo thành một vùng khép kín. Ví dụ,```
1 1
/
```có câu trả lời`0`. Đoạn chéo đơn chạy từ góc ranh giới này sang góc ranh giới khác nên hai bên vẫn nối với bên ngoài. Việc triển khai bất cẩn coi mỗi dấu gạch chéo là tạo một ô kèm theo riêng biệt sẽ trả về câu trả lời sai. 

Sự sắp xếp ngược lại cũng có vấn đề:```
2 2
\/
/\
```có câu trả lời`0`. Bốn đoạn đường chéo không bao quanh vùng bên trong theo hướng này. Việc triển khai giả định mọi nhóm dấu gạch chéo dày đặc tạo ra một vùng kín có thể trả về không chính xác`1`. 

Cuối cùng, một mê cung hoàn toàn mở như```
1 1
.
```đã bao gồm một vùng, cụ thể là vùng được kết nối bên ngoài, vì vậy câu trả lời là`0`. Điều này phát hiện các triển khai vô tình đếm số lượng ô con trống thay vì số lượng thành phần được kết nối. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử loại bỏ các bức tường và kiểm tra xem liệu tất cả các khu vực có thể tiếp cận bên ngoài hay không. Đối với mỗi bức tường ứng cử viên, chúng tôi có thể tạm thời loại bỏ nó, chạy lũ tràn qua mê cung và kiểm tra xem mọi khu vực tự do đã được kết nối với bên ngoài hay chưa. Có thể có (rc) bức tường, trong khi kiểm tra kết nối chạm vào các vị trí mê cung (O(rc)), vì vậy trường hợp xấu nhất là (O((rc)^2)). Tại (r=c=1000), đó là thứ tự của (10^{12}) phép tính. Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng khả năng kết nối sau mỗi thay đổi có thể xảy ra, nhưng các tìm kiếm lặp đi lặp lại khiến nó không thể sử dụng được. 

Quan sát quan trọng là chúng ta không bao giờ cần phải quyết định loại bỏ những bức tường cụ thể nào. Nếu mê cung ban đầu có (k) vùng không gian tự do được kết nối thì chính xác (k-1) bức tường là đủ. Loại bỏ một bức tường có thể hợp nhất hai vùng, giảm số lượng thành phần đi một. Lặp lại điều này cho đến khi chỉ còn lại vùng được kết nối bên ngoài sẽ loại bỏ (k-1) và không có giải pháp nào có thể sử dụng ít hơn. 

Những gì còn lại là đếm chính xác các khu vực đó. Việc biểu diễn đường chéo làm cho điều đó trở nên bất tiện trên lưới ký tự ban đầu (r \times c) vì chuyển động xảy ra xung quanh các bức tường chéo. Chúng ta có thể loại bỏ sự phức tạp hình học đó bằng cách chia tỷ lệ biểu diễn theo hai hướng. Đây là phép biến đổi tiêu chuẩn được mô tả trong phân tích cuộc thi: mỗi ký tự đầu vào trở thành một khối (2 \times 2), với hai ô dọc theo đường chéo tương ứng được đánh dấu là các bức tường và một ranh giới trống được thêm vào xung quanh toàn bộ mê cung. 

Vì`/`, các ô bị chặn là các ô phía trên bên phải và phía dưới bên trái của khối (2 \times 2) của nó. Vì`\`, chúng là các ô phía trên bên trái và phía dưới bên phải. Một dấu chấm để trống cả bốn ô. Chuyển động trong lưới mở rộng là chuyển động bốn hướng thông thường, vì vậy các bức tường chéo giờ đây hoạt động giống hệt như các rào cản vật lý. 

Ranh giới trống thêm đại diện cho vô hạn bên ngoài. Nó đảm bảo rằng mọi khu vực có thể thoát khỏi mê cung đều được kết nối với một thành phần chung. Sau đó, chúng tôi có thể lấp đầy lưới mở rộng và đếm các thành phần được kết nối của nó. Câu trả lời là số thành phần trừ đi một vì một thành phần nằm ở bên ngoài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((rc)^2)) | (O(rc)) | Quá chậm | 
| Lấp đầy lưới điện mở rộng | (O(rc)) | (O(rc)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới ký tự (r \times c) và tạo một lưới mở rộng có kích thước ((2r+2) \times (2c+2)). Đường viền một ô bổ sung được cố tình để trống để nó tượng trưng cho phần bên ngoài của mê cung. 
2. Ánh xạ mọi ký tự gốc vào khối (2 \times 2) của nó. Đối với dấu chấm, hãy để trống cả bốn vị trí vì không có tường. Vì`/`, đánh dấu các vị trí phía trên bên phải và phía dưới bên trái là các bức tường. Vì`\`, đánh dấu các vị trí phía trên bên trái và phía dưới bên phải là các bức tường. Chia tỷ lệ thành hai giúp bức tường chéo có đủ độ dày mà chuyển động bốn hướng thông thường không thể vượt qua nó. 
3. Quét lưới mở rộng. Bất cứ khi nào tìm thấy một vị trí trống chưa được ghé thăm, hãy bắt đầu lấp đầy từ vị trí đó và đánh dấu mọi vị trí trống có thể tiếp cận là đã ghé thăm. Tăng số lượng thành phần một lần cho mỗi lần lấp lũ mới. 
4. Coi thành phần chứa ranh giới được thêm vào là vùng bên ngoài. Mọi thành phần khác là một vùng khép kín hoặc bị ngắt kết nối cần được kết nối với bên ngoài. 
5. In`components - 1`. Phép trừ sẽ loại bỏ thành phần bên ngoài khỏi số đếm. 

### Tại sao nó hoạt động 

Lưới mở rộng bảo toàn chính xác khả năng kết nối của mê cung ban đầu. Một con đường trong mê cung ban đầu tương ứng với một con đường bốn hướng xuyên qua các ô tự do trong biểu diễn mở rộng, trong khi mỗi bức tường chéo chiếm hai vị trí cần thiết để chặn lối đi ngang qua nó. Ranh giới trống kết nối tất cả các cách rời mê cung vào một thành phần bên ngoài. 

Giả sử lưới mở rộng có (k) thành phần được kết nối. Một trong số đó là các vùng bên ngoài, để lại (k-1) các vùng phải được kết nối với nó. Việc loại bỏ một bức tường có thể hợp nhất tối đa hai thành phần, do đó, việc loại bỏ ít hơn (k-1) là không đủ. Mặt khác, do mỗi thành phần không phải bên ngoài được ngăn cách với phần còn lại bằng tường nên việc loại bỏ một bức tường ngăn cách phù hợp có thể hợp nhất nó với thành phần khác và lặp lại thao tác này sẽ kết nối tất cả (k) thành phần bằng cách loại bỏ chính xác (k-1). Vì vậy, mức tối thiểu chính xác là số lượng thành phần trừ đi một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    r, c = map(int, input().split())
    maze = [input().strip() for _ in range(r)]

    h = 2 * r + 2
    w = 2 * c + 2

    # 0 = free, 1 = wall/visited
    grid = bytearray(h * w)

    # Convert every slash into two blocked cells in its 2x2 block.
    for i in range(r):
        row = maze[i]
        base = (2 * i + 1) * w + 1

        for j, ch in enumerate(row):
            p = base + 2 * j

            if ch == '/':
                grid[p + 1] = 1
                grid[p + w] = 1
            elif ch == '\\':
                grid[p] = 1
                grid[p + w + 1] = 1

    components = 0

    # The stack stores linear indices in the expanded grid.
    stack = []

    for i in range(h):
        start = i * w

        for j in range(w):
            pos = start + j

            if grid[pos]:
                continue

            components += 1
            grid[pos] = 1
            stack.append(pos)

            while stack:
                cur = stack.pop()

                nxt = cur - w
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur + w
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur - 1
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur + 1
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

    print(components - 1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai lưu trữ các hàng ban đầu và phân bổ lưới mở rộng thành một`bytearray`. Mảng byte phẳng có hiệu suất bộ nhớ cao hơn đáng kể so với danh sách danh sách Python, điều này quan trọng vì lưới mở rộng có thể chứa khoảng bốn triệu vị trí khi cả hai chiều đều là 1000. 

biểu thức`p = base + 2 * j`xác định ô phía trên bên trái của khối (2 \times 2) tương ứng với ký tự gốc tại`(i, j)`. Vì`/`, các vị trí bị chặn là`p + 1`Và`p + w`. Vì`\`, họ là`p`Và`p + w + 1`. Đây chính xác là hai ô nằm trên đường chéo thích hợp. 

Ranh giới không bao giờ được đánh dấu rõ ràng là một bức tường, vì vậy toàn bộ khung bên ngoài bắt đầu dưới dạng không gian trống. Nó trở thành thành phần bên ngoài trong quá trình lấp lũ. 

Việc lấp lũ sử dụng các chỉ số tuyến tính thay vì`(row, column)`bộ dữ liệu. Di chuyển theo chiều dọc thay đổi chỉ mục bằng cách`w`, trong khi di chuyển theo chiều ngang sẽ thay đổi nó từng cái một. Lưới mở rộng có đường viền hoàn toàn trống, do đó việc truy cập vào`cur - w`,`cur + w`,`cur - 1`, Và`cur + 1`vẫn còn hiệu lực. Đường viền một ô cũng hữu ích cho việc di chuyển theo chiều ngang vì ngay cả sự chuyển đổi gây ra bởi việc lập chỉ mục phẳng ở ranh giới hàng bên ngoài vẫn nằm trong khu vực bên ngoài được thể hiện rõ ràng. 

Một vị trí được đánh dấu là đã truy cập bằng cách thay đổi byte của nó từ`0`ĐẾN`1`. Giá trị byte giống nhau được sử dụng cho các bức tường và các ô đã ghé thăm vì cả hai đều có nghĩa là vùng lấp lũ không được vào lại vị trí đó. Việc đánh dấu một ô trước khi đẩy nó vào ngăn xếp sẽ ngăn chặn việc chèn cùng một ô nhiều lần. 

Không có đệ quy được sử dụng. DFS đệ quy sẽ cần độ sâu đệ quy tỷ lệ thuận với số lượng ô được mở rộng và sẽ vượt quá giới hạn đệ quy của Python trên các mê cung mở lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
/\
\/
```Sau khi mở rộng, bốn bức tường chéo tạo thành một khu vực khép kín bên trong. Ranh giới bên ngoài là một khu vực khác. 

| Bước | Hành động hiện tại | Linh kiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Xây dựng lưới mở rộng (6 \times 6) | 0 | Chém trở thành rào cản chéo | 
| 2 | Quét tới ranh giới bên ngoài | 1 | Lũ lụt đánh dấu toàn bộ bên ngoài | 
| 3 | Quét đến vùng tự do kèm theo | 2 | Lũ lụt lần thứ hai đánh dấu khu vực kèm theo | 
| 4 | Kết thúc quá trình quét | 2 | Không còn thành phần miễn phí nữa | 
| 5 | Trừ thành phần bên ngoài | 1 | Số lần dỡ bỏ tường tối thiểu =`2 - 1`| 

Kết quả là`1`. Điều này chứng tỏ tại sao câu trả lời là số lượng thành phần chứ không phải số lượng bức tường. Bốn bức tường có thể tạo thành một khu vực khép kín và chỉ cần loại bỏ một trong số chúng. 

### Mẫu 2 

Đầu vào là```
4 4
/\..
\.\.
.\/\
..\/
```Biểu diễn mở rộng chứa ba thành phần không gian trống, một trong số đó là bên ngoài. 

| Bước | Hành động hiện tại | Linh kiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Xây dựng lưới mở rộng (10 \times 10) | 0 | Tất cả các bức tường chéo được thể hiện rõ ràng | 
| 2 | Đã đạt được ô trống chưa được truy cập đầu tiên | 1 | Đánh dấu lũ lụt bên ngoài | 
| 3 | Một vùng kèm theo riêng biệt được tìm thấy | 2 | Thành phần thứ hai được đánh dấu | 
| 4 | Một khu vực riêng biệt khác được tìm thấy | 3 | Thành phần thứ ba được đánh dấu | 
| 5 | Kết thúc quá trình quét | 3 | Tất cả các vị trí miễn phí thuộc về một trong ba thành phần | 
| 6 | Trừ thành phần bên ngoài | 2 | Số lần dỡ bỏ tường tối thiểu =`3 - 1`| 

Kết quả là`2`. Dấu vết thể hiện tính bất biến trung tâm: sau khi quá trình lấp lũ kết thúc, mọi vị trí trống trong thành phần đó đã được phân loại vĩnh viễn, do đó quá trình quét sau này không thể vô tình đếm cùng một vùng hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(rc)) | Lưới mở rộng có các ô ((2r+2)(2c+2)=O(rc)) và mỗi ô được xử lý nhiều nhất một lần | 
| Không gian | (O(rc)) | Lưới mở rộng và ngăn xếp DFS chứa các mục (O(rc)) | 

Đối với (r,c \le 1000), lưới mở rộng chỉ chứa hơn bốn triệu vị trí. Mỗi vị trí được khởi tạo một lần và được truy cập nhiều nhất một lần, do đó thuật toán sẽ chia tỷ lệ tuyến tính với kích thước đầu vào ban đầu. Căn hộ`bytearray`cách biểu diễn giữ cho lưới đủ nhỏ gọn cho giới hạn bộ nhớ của cuộc thi, trong khi DFS lặp sẽ tránh được chi phí đệ quy và lỗi độ sâu đệ quy. Tài liệu chính thức của cuộc thi đưa ra giới hạn thời gian là 5 giây và giới hạn bộ nhớ là 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(data: str) -> str:
    inp = io.StringIO(data)

    r, c = map(int, inp.readline().split())
    maze = [inp.readline().strip() for _ in range(r)]

    h = 2 * r + 2
    w = 2 * c + 2

    grid = bytearray(h * w)

    for i in range(r):
        row = maze[i]
        base = (2 * i + 1) * w + 1

        for j, ch in enumerate(row):
            p = base + 2 * j

            if ch == '/':
                grid[p + 1] = 1
                grid[p + w] = 1
            elif ch == '\\':
                grid[p] = 1
                grid[p + w + 1] = 1

    components = 0
    stack = []

    for i in range(h):
        start = i * w

        for j in range(w):
            pos = start + j

            if grid[pos]:
                continue

            components += 1
            grid[pos] = 1
            stack.append(pos)

            while stack:
                cur = stack.pop()

                nxt = cur - w
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur + w
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur - 1
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

                nxt = cur + 1
                if grid[nxt] == 0:
                    grid[nxt] = 1
                    stack.append(nxt)

    return str(components - 1) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run("""\
2 2
/\\
\\/
""") == "1\n", "sample 1"

# Provided sample 2
assert run("""\
4 4
/\\..
\\.\\.
.\\/\\
..\\/
""") == "2\n", "sample 2"

# Provided sample 3
assert run("""\
2 2
\\/
/\\
""") == "0\n", "sample 3"

# Provided sample 4
assert run("""\
8 20
/\\/\\/\\/\\/\\/\\/\\/\\/\\/\\
\\../\\.\\/./././\\/\\/\\
/./\\.././\\/\\.\\/\\/\\/\\
\\/\\/\\.\\/\\/./\\/..\\../
/\\/./\\/\\/./..\\/\\/..\\
\\.\\.././\\.\\/\\/./\\.\\/
 /.../\\../..\\/./.../\\
\\/\\/\\/\\/\\/\\/\\/\\/\\/\\/ 
""".replace(" \n", "\n")) == "26\n", "sample 4"

# Minimum-size, all-open maze
assert run("""\
1 1
.
""") == "0\n", "minimum-size open maze"

# Boundary condition: a single diagonal wall does not enclose anything
assert run("""\
1 1
/
""") == "0\n", "single boundary slash"

# Boundary and horizontal adjacency case
assert run("""\
1 2
/\\
""") == "0\n", "two boundary-touching diagonals"

# Maximum-size, all-equal input
max_input = "1000 1000\n" + (". " * 1000).replace(" ", "") + "\n"
max_input = "1000 1000\n" + ".\n" * 1000
assert run(max_input) == "0\n", "maximum-size all-open maze"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`với`.`|`0`| Kích thước tối thiểu và đã được kết nối bên ngoài | 
|`1 1`với`/`|`0`| Một đường chéo chạm vào ranh giới không tạo ra một vùng khép kín | 
|`1 2`với`/\`|`0`| Đường chéo ranh giới liền kề và xử lý tọa độ | 
|`1000 1000`với tất cả`.`|`0`| Kích thước tối đa, đầu vào hoàn toàn bằng nhau và chia tỷ lệ tuyến tính | 

Mẫu thứ tư được cung cấp được sao chép từ tuyên bố cuộc thi. Việc chuẩn hóa khoảng trắng trong khai thác thử nghiệm chỉ loại bỏ các khoảng trắng ở cuối ngẫu nhiên khỏi chữ nhiều dòng; nó không làm thay đổi bất kỳ đặc điểm mê cung nào. 

## Vỏ cạnh 

Đối với mê cung mở nhỏ nhất,```
1 1
.
```lưới mở rộng là một lưới trống (4 \times 4). Đường biên và ô gốc duy nhất đều được kết nối với nhau, do đó việc lấp đầy sẽ tìm thấy chính xác một thành phần. Thuật toán in`1 - 1 = 0`. 

Đối với một bức tường chéo đơn,```
1 1
/
```dấu gạch chéo chiếm các ô phía trên bên phải và phía dưới bên trái của khối (2 \times 2) của nó. Các ô tự do còn lại vẫn kết nối xung quanh các đầu của đường chéo với ranh giới trống bên ngoài. Việc lấp lũ lại tìm thấy chính xác một thành phần, đưa ra`0`. Điểm mấu chốt là việc chạm vào ranh giới là không đủ để bao quanh một khu vực. 

Đối với cách sắp xếp ngược lại (2 \times 2),```
2 2
\/
/\
```các đoạn chéo không tạo ra một vùng bên trong khép kín. Biểu diễn mở rộng có một thành phần không gian trống chứa bên ngoài, do đó số lượng thành phần là một và câu trả lời là 0. Đây là lý do tại sao thuật toán tính kết nối thực tế thay vì dựa vào số lượng hoặc mật độ ký tự gạch chéo. 

Đối với sự sắp xếp kèm theo,```
2 2
/\
\/
```biểu diễn mở rộng có hai thành phần. Đợt lũ đầu tiên tràn tới ranh giới bên ngoài, trong khi đợt lũ thứ hai vẫn bị mắc kẹt bên trong các bức tường chéo. Vì việc loại bỏ một bức tường có thể kết nối hai thành phần này và không có giải pháp nào có thể loại bỏ bằng 0 bức tường nên câu trả lời chính xác là một. 

Đối với một mê cung mở có kích thước tối đa,```
1000 1000
...................................................................
...
```với 1000 hàng 1000 chấm thì không có bức tường nào cả. Mọi vị trí mở rộng đều thuộc về cùng một thành phần được kết nối bên ngoài. Quá trình quét thực hiện công việc tuyến tính trên khoảng bốn triệu ô và trả về số 0. Trường hợp này thực hiện sử dụng dấu chân bộ nhớ và xác nhận rằng việc triển khai không phụ thuộc vào sự hiện diện của bất kỳ bức tường nào.
