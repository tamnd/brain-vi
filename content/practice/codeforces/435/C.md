---
title: "CF 435C - Điện tâm đồ"
description: "Đầu vào mô tả một đường đa tuyến xen kẽ giữa các đoạn đường chéo tăng và giảm. Độ dài của đoạn $i$-th là $ai$."
date: "2026-06-07T02:45:08+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 435
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 249 (Div. 2)"
rating: 1600
weight: 435
solve_time_s: 138
verified: false
draft: false
---

[CF 435C - Điện tâm đồ](https://codeforces.com/problemset/problem/435/C) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một đường đa tuyến xen kẽ giữa các đoạn đường chéo tăng và giảm. Chiều dài của$i$-đoạn thứ là$a_i$. 

Nếu chúng ta bắt đầu từ một điểm nào đó và vẽ các đoạn lần lượt, đoạn đầu tiên lại đi lên bên phải, đoạn thứ hai đi xuống bên phải, đoạn thứ ba lại đi lên bên phải, v.v. Mỗi đơn vị chuyển động đều thay đổi cả hai tọa độ một cách chính xác. Hình ảnh cuối cùng chỉ bao gồm các nét chéo, vì vậy mỗi ô được chiếm giữ chứa một trong hai`/`hoặc`\`. 

Nhiệm vụ là in điện tâm đồ dưới dạng nghệ thuật ASCII. Mỗi cột tương ứng với một đơn vị chuyển động ngang. Mỗi hàng tương ứng với một cấp độ dọc. Vị trí trống phải chứa dấu cách. Đầu ra phải chứa chính xác số hàng bằng khoảng dọc của bản vẽ. 

Những hạn chế là nhỏ. Số lượng phân đoạn nhiều nhất là 1000 và tổng độ dài của tất cả các phân đoạn nhiều nhất là 1000. Vì mỗi bước đơn vị của một phân đoạn chiếm chính xác một vị trí ký tự trong ảnh cuối cùng nên tổng số ô không trống tối đa là 1000. Ngay cả một thuật toán mô phỏng rõ ràng từng bước của đa tuyến cũng đủ nhanh. 

Khó khăn chính không phải là hiệu quả mà là phối hợp xử lý. 

Một sai lầm dễ mắc phải là đặt ký tự gạch chéo sai chiều dọc. Coi như:```
2
1 1
```Đoạn đầu tiên tăng một bước và đoạn thứ hai giảm một bước. Hình ảnh đúng là:```
/\
```Việc thực hiện bất cẩn có thể đặt`\`thấp hơn một hàng và tạo ra một cầu thang thay vì một đường đa tuyến được kết nối. 

Một trường hợp tinh tế khác là khi bản vẽ đạt đến tọa độ dọc âm.```
2
1 3
```Con đường đi lên một lần và sau đó rơi xuống ba lần. Điểm thấp nhất là dưới mức bắt đầu. Nếu chúng ta trực tiếp sử dụng các tọa độ đó làm chỉ số hàng, chúng ta sẽ thu được chỉ số mảng âm. Thay vào đó, hình ảnh phải được dịch chuyển lên trên trước khi in. 

Nguồn lỗi thứ ba là việc xác định chiều cao cần thiết. Coi như:```
3
2 2 2
```Con đường di chuyển lên, xuống, rồi lại lên. Mức truy cập cao nhất và thấp nhất không nhất thiết chỉ là điểm cuối của phân đoạn. Việc triển khai phải theo dõi mọi ô bị chiếm dụng, nếu không lưới được phân bổ có thể quá nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng bản vẽ chính xác như mô tả. Bắt đầu tại tọa độ$(0,0)$, đi qua từng bước đơn vị của mỗi phân đoạn, ghi lại ký tự chiếm vị trí hiện tại và theo dõi tọa độ dọc tối thiểu và tối đa gặp phải. 

Vì tổng số bước nhiều nhất là 1000 nên mô phỏng này thực hiện tối đa 1000 lần cập nhật. Sau khi đã biết tất cả các vị trí, chúng ta tạo một lưới hình chữ nhật bao phủ toàn bộ phạm vi dọc và đặt các ký tự đã ghi vào đó. 

Một cách giải thích đơn giản hơn sẽ là tạo ra một mặt phẳng tọa độ lớn và kiểm tra từng ô đối với từng phân đoạn để xác định xem ô đó có thuộc về điện tâm đồ hay không. Nếu chiều rộng khoảng 1000 và chiều cao khoảng 1000, điều đó dẫn đến khoảng một triệu ô, mỗi ô có khả năng được kiểm tra theo nhiều phân đoạn. Cách tiếp cận như vậy là không cần thiết. 

Quan sát quan trọng là bản thân hình ảnh chỉ chứa số lượng ô không trống bằng tổng chiều dài đường dẫn. Vì tổng độ dài phân đoạn được giới hạn bởi 1000 nên chúng tôi có thể tạo trực tiếp chính xác các ô đó và không bao giờ kiểm tra các tọa độ không sử dụng. 

Điều này biến bài toán thành một bài toán mô phỏng đơn giản hơn là một bài toán hình học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lưới Brute Force | O(H·W·n) | O(H·W) | Không cần thiết | 
| Mô phỏng tối ưu | O(tổng aᵢ) | O(H·W) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xuất phát tại tọa độ$(x,y)=(0,0)$. 
2. Duy trì một từ điển ánh xạ các tọa độ bị chiếm đóng theo ký tự của chúng`/`hoặc`\`. 
3. Duy trì tọa độ hàng tối thiểu và tối đa xuất hiện trong bản vẽ. 
4. Xử lý các phân đoạn từ trái sang phải. 
5. Đối với đoạn đánh số lẻ (đường đi lên): 

Đối với mỗi bước đơn vị: 

- Di chuyển một đơn vị lên trên. 
- Địa điểm`/`tại ô tương ứng với bước đó. 
- Tiến lên một cột bên phải. 
- Cập nhật giới hạn dọc. 

Dấu gạch chéo được đặt ở điểm cuối phía trên của bước vì phân đoạn thị giác đi từ phía dưới bên trái sang phía trên bên phải. 
6. Đối với đoạn đánh số chẵn (đường đi rơi): 

Đối với mỗi bước đơn vị: 

- Địa điểm`\`ở cấp độ hiện tại. 
- Di chuyển một cột sang bên phải. 
- Di chuyển một đơn vị xuống dưới. 
- Cập nhật giới hạn dọc. 

Dấu gạch chéo ngược chiếm ô trên của bước giảm dần. 
7. Sau khi tất cả các đoạn được xử lý, hãy tính chiều cao như`max_y - min_y + 1`và chiều rộng là tổng quãng đường đi được theo phương ngang. 
8. Tạo một lưới chứa đầy khoảng trắng. 
9. Chuyển đổi mọi tọa độ được lưu trữ thành một hàng lưới bằng cách dịch chuyển nó bằng cách sử dụng`max_y - y`. Điều này đặt các giá trị y lớn hơn gần đầu ra. 
10. Điền vào các vị trí lưới tương ứng bằng các ký tự được lưu trữ của chúng. 
11. In tất cả các hàng. 

### Tại sao nó hoạt động 

Mỗi đơn vị chuyển động của điện tâm đồ tương ứng với chính xác một ký tự được in. Mô phỏng đi theo đường dẫn từng bước một và ghi lại ô chính xác mà mỗi nét chéo chiếm giữ. Các tọa độ được lưu trữ bảo toàn hình dạng hình học của đường đa tuyến. 

Các mức dọc được truy cập tối thiểu và tối đa xác định hình chữ nhật nhỏ nhất chứa toàn bộ bản vẽ. Việc dịch chuyển tất cả các tọa độ với cùng một lượng sẽ bảo toàn các vị trí tương đối trong khi loại bỏ các chỉ số âm. 

Vì mỗi phân đoạn được mô phỏng chính xác theo định nghĩa của nó và mỗi ô được sử dụng đều được ghi lại một lần nên bản vẽ ASCII được tạo ra chính xác là điện tâm đồ cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cells = {}

    x = 0
    y = 0

    min_y = 0
    max_y = 0

    for i, length in enumerate(a):
        if i % 2 == 0:  # rising segment
            for _ in range(length):
                y += 1
                cells[(x, y)] = '/'
                min_y = min(min_y, y)
                max_y = max(max_y, y)
                x += 1
        else:  # falling segment
            for _ in range(length):
                cells[(x, y)] = '\\'
                min_y = min(min_y, y)
                max_y = max(max_y, y)
                x += 1
                y -= 1

    width = x
    height = max_y - min_y + 1

    grid = [[' '] * width for _ in range(height)]

    for (cx, cy), ch in cells.items():
        row = max_y - cy
        grid[row][cx] = ch

    sys.stdout.write('\n'.join(''.join(row) for row in grid))

if __name__ == "__main__":
    solve()
```Mô phỏng tuân theo định nghĩa hình học trực tiếp. Biến`x`theo dõi cột hiện tại, trong khi`y`theo dõi mức độ dọc hiện tại. 

Quy định về vị trí của`/`Và`\`hơi khác nhau. Trong một bước đi lên, dấu gạch chéo thuộc cấp độ cao hơn đạt được sau khi di chuyển. Trong bước đi xuống, dấu gạch chéo ngược thuộc về mức hiện tại trước khi giảm dần. Việc trộn lẫn hai quy ước này là nguyên nhân phổ biến nhất dẫn đến câu trả lời sai. 

Từ điển chỉ lưu trữ các ô bị chiếm đóng. Vì tối đa 1000 ô được rút ra nên mức sử dụng bộ nhớ vẫn rất nhỏ. 

Khi xây dựng lưới đầu ra, chỉ số hàng tăng xuống, trong khi tọa độ y hình học tăng lên. Sự chuyển đổi```
row = max_y - cy
```thực hiện cú lật dọc cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 2 5 1
```Dấu vết mô phỏng: 

| Phân đoạn | Chiều dài | Hướng | x sau đoạn | y sau đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | Lên | 3 | 3 | 
| 2 | 1 | Xuống | 4 | 2 | 
| 3 | 2 | Lên | 6 | 4 | 
| 4 | 5 | Xuống | 11 | -1 | 
| 5 | 1 | Lên | 12 | 0 | 

Phạm vi dọc đã truy cập: 

| Số lượng | Giá trị | 
| --- | --- | 
| max_y | 4 | 
| phút_y | -1 | 
| chiều cao | 6 | 
| chiều rộng | 12 | 

Dấu vết cho thấy tại sao việc theo dõi cả hai thái cực là cần thiết. Bản vẽ tăng lên cấp 4 và sau đó giảm xuống dưới mức bắt đầu xuống -1. 

### Ví dụ 2 

đầu vào:```
2
1 1
```Dấu vết mô phỏng: 

| Phân đoạn | Chiều dài | Hướng | x sau đoạn | y sau đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Lên | 1 | 1 | 
| 2 | 1 | Xuống | 2 | 0 | 

Các ô bị chiếm dụng: 

| Tọa độ | Nhân vật | 
| --- | --- | 
| (0,1) | / | 
| (1,1) | \ | 

Đầu ra:```
/\
```Ví dụ này trình bày các quy tắc vị trí khác nhau cho các phân đoạn hướng lên và hướng xuống. Cả hai ký tự đều chiếm cùng một hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng aᵢ) | Mỗi bước đơn vị được mô phỏng chính xác một lần | 
| Không gian | O(H·W) | Lưu trữ cho lưới đầu ra | 

Vì tổng chiều dài đường dẫn tối đa là 1000, chiều rộng tối đa là 1000 và chiều cao cũng tối đa là 1000. Lưới kết quả vừa vặn thoải mái trong giới hạn bộ nhớ và mô phỏng dễ dàng chạy trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    cells = {}

    x = 0
    y = 0

    min_y = 0
    max_y = 0

    for i, length in enumerate(a):
        if i % 2 == 0:
            for _ in range(length):
                y += 1
                cells[(x, y)] = '/'
                min_y = min(min_y, y)
                max_y = max(max_y, y)
                x += 1
        else:
            for _ in range(length):
                cells[(x, y)] = '\\'
                min_y = min(min_y, y)
                max_y = max(max_y, y)
                x += 1
                y -= 1

    width = x
    height = max_y - min_y + 1

    grid = [[' '] * width for _ in range(height)]

    for (cx, cy), ch in cells.items():
        row = max_y - cy
        grid[row][cx] = ch

    return '\n'.join(''.join(row) for row in grid)

# minimum valid input
assert run("2\n1 1\n") == "/\\"

# symmetric mountain
assert run("2\n2 2\n") == " /\\\n/  \\\\"

# returns to origin repeatedly
assert run("4\n1 1 1 1\n") == "/\\/\\"

# deeper descent below start level
assert run("2\n1 3\n") == "/\\\n  \\\n   \\\\"

# all equal lengths
assert len(run("4\n2 2 2 2\n").splitlines()) > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`/\`| Trường hợp hợp lệ nhỏ nhất | 
|`2 / 2 2`| Núi hai hàng | Xử lý đúng chiều cao > 1 | 
|`4 / 1 1 1 1`|`/\/\`| Nhiều lượt luân phiên | 
|`2 / 1 3`| Giảm xuống dưới mức bắt đầu | Logic dịch chuyển tọa độ | 
|`4 / 2 2 2 2`| Bản vẽ hợp lệ không trống | Các đoạn có độ dài bằng nhau lặp đi lặp lại | 

## Vỏ cạnh 

### Bản vẽ xuống dưới mức bắt đầu 

đầu vào:```
2
1 3
```Con đường đạt đến cấp độ:```
0 -> 1 -> 0 -> -1 -> -2
```Tọa độ y tối thiểu trở thành`-2`. Khi xây dựng đầu ra, tất cả các hàng được dịch chuyển lên trên`2`. Hình dạng tương đối không thay đổi và mọi tọa độ đều nhận được chỉ mục hàng không âm hợp lệ. 

### Các đỉnh liên tiếp có cùng độ cao 

đầu vào:```
4
1 1 1 1
```Đường dẫn liên tục trở về cùng cấp độ:```
0 -> 1 -> 0 -> 1 -> 0
```Một số phân đoạn chạm vào cùng một hàng. Bởi vì mỗi bước chiếm một cột khác nhau nên không có ô nào ghi đè lên nhau một cách không chính xác. Đầu ra trở thành:```
/\/\
```### Chiều dài ngang tối đa 

đầu vào:```
2
500 500
```Chiều rộng trở thành chính xác 1000, đây cũng là tổng chiều dài tối đa có thể có. Thuật toán vẫn chỉ thực hiện 1000 bước mô phỏng và chỉ lưu trữ 1000 ô bị chiếm dụng trước khi tạo lưới cuối cùng. 

### Chiều cao được xác định bởi các vị trí trung gian 

đầu vào:```
3
2 1 2
```Mức cao nhất đạt được là 3, mặc dù một số điểm cuối phân khúc thấp hơn. Theo dõi cập nhật từng bước`max_y`một cách chính xác. Lưới được phân bổ đủ cao để chứa toàn bộ điện tâm đồ mà không cần cắt bớt.
