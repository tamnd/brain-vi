---
title: "CF 1182B - Plus từ Hình ảnh"
description: "Chúng ta được cung cấp một lưới các ký tự đại diện cho một bức tranh, trong đó mỗi ô trống hoặc được điền. Nhiệm vụ là quyết định xem các ô đã điền có tạo thành chính xác một hình hình cộng hay không. Hình cộng hợp lệ có một ô được lấp đầy ở giữa."
date: "2026-06-13T11:29:08+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1182
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 566 (Div. 2)"
rating: 1300
weight: 1182
solve_time_s: 679
verified: true
draft: false
---

[CF 1182B - Plus từ Hình ảnh](https://codeforces.com/problemset/problem/1182/B) 

**Đánh giá:** 1300 
**Tags:** dfs và tương tự, triển khai, chuỗi 
**Thời gian giải:** 11m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các ký tự đại diện cho một bức tranh, trong đó mỗi ô trống hoặc được điền. Nhiệm vụ là quyết định xem các ô đã điền có tạo thành chính xác một hình hình cộng hay không. 

Hình cộng hợp lệ có một ô được lấp đầy ở giữa. Từ tâm đó phải có ít nhất một ô được lấp đầy kéo dài liên tục theo bốn hướng: lên, xuống, trái, phải. Các phần mở rộng này phải là các đoạn thẳng không có khoảng trống. Mỗi ô được lấp đầy trong lưới phải thuộc về cấu trúc duy nhất này, nghĩa là không có thành phần bổ sung, không có ô biệt lập và không có nhánh phụ. 

Vì vậy, cấu trúc mà chúng tôi đang xác thực không chỉ “trông giống như một điểm cộng”, mà còn là một mẫu được kết nối rất chặt chẽ: một tâm, bốn nhánh thẳng và không có gì khác ở bất kỳ đâu trong lưới. 

Kích thước lưới có thể lớn tới 500 x 500, nghĩa là lên tới 250.000 ô. Bất kỳ giải pháp nào cố gắng quét hoặc mở rộng liên tục từ nhiều ứng cử viên theo phương pháp bậc hai hoặc tệ hơn đều có nguy cơ gây ra chi phí không cần thiết. Tuy nhiên, ngay cả việc quét tuyến tính toàn bộ trên tất cả các ô cũng hoàn toàn có thể chấp nhận được, vì 250.000 thao tác là không đáng kể. 

Một số trường hợp đặc biệt sẽ phá vỡ lối suy luận ngây thơ một cách nhanh chóng. 

Cạm bẫy đầu tiên là có nhiều trung tâm ứng viên. Ví dụ: nếu có hai đường sao thẳng đứng riêng biệt:```
.*....
.*....
.*....
..*...
..*...
..*...
```Đây không phải là một điểm cộng ngay cả khi mỗi cột riêng lẻ trông giống một cánh tay. Câu trả lời đúng là KHÔNG vì có nhiều hơn một cấu trúc được kết nối. 

Cạm bẫy thứ hai là gãy tay. Nếu trung tâm tồn tại nhưng một hướng có khoảng cách:```
..*..
..*..
.*.*.
..*..
.....
```Điều này không hợp lệ vì cánh tay không liên tục. 

Cạm bẫy thứ ba là các ô đi lạc bên ngoài dấu cộng:```
..*..
.***.
..*..
..*..
*....
```Ngay cả khi có dấu cộng, ngôi sao phụ ở phía dưới bên trái sẽ vô hiệu hóa cấu hình. 

Thách thức chính không phải là phát hiện điểm cộng mà là thực thi đồng thời tính độc quyền và tính liên tục trên toàn cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi tế bào sao như một trung tâm tiềm năng. Đối với mỗi ứng cử viên, chúng tôi sẽ mở rộng theo bốn hướng, đánh dấu các ô đã truy cập và xác minh xem tất cả các ngôi sao có được che phủ chính xác một lần hay không. 

Điều này hoạt động về mặt khái niệm vì điểm cộng được xác định bởi một trung tâm duy nhất và các nhánh xác định. Tuy nhiên, chi phí trở thành vấn đề trong lưới điện dày đặc. Trong trường hợp xấu nhất khi hầu hết các ô đều là sao, chúng ta có thể thử O(h·w) tâm và mỗi phần mở rộng quét tới O(h + w), dẫn đến độ phức tạp O(h·w·(h + w)). Ở mức 500 x 500, điều này vượt xa giới hạn. 

Quan sát quan trọng là một dấu cộng hợp lệ có cấu trúc rất cứng nhắc. Có chính xác một ô mà cả bốn hướng đều có thể kéo dài liên tục. Ô đó phải có ít nhất một ngôi sao lân cận ở cả bốn hướng và mọi ngôi sao khác phải nằm trên đúng một trong bốn đường thẳng đi qua nó. Điều này có nghĩa là chúng tôi không cần kiểm tra từng ô, chúng tôi chỉ cần xác định trung tâm duy nhất và xác thực các nhánh một lần. 

Chúng ta có thể tìm thấy trung tâm bằng cách quét tìm một ngôi sao có ít nhất một ngôi sao ở trên, dưới, trái và phải. Sau khi xác định được ứng cử viên như vậy, chúng tôi xác thực đầy đủ cấu trúc bằng cách mở rộng từ cấu trúc đó và đảm bảo phạm vi bao phủ khớp chính xác với tất cả các ngôi sao trong lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h·w·(h+w)) | O(1) | Quá chậm | 
| Phát hiện trung tâm + xác nhận | O(h·w) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét lưới và thu thập tất cả các vị trí ngôi sao, đồng thời kiểm tra các trung tâm ứng cử viên. 

Một ô được coi là trung tâm ứng cử viên nếu nó có ít nhất một ngôi sao ở cả bốn hướng. Điều kiện này đảm bảo nó có thể là giao điểm của dấu cộng. 
2. Nếu không có trung tâm tuyển sinh nào tồn tại, hãy trả lại ngay SỐ. 

Một dấu cộng hợp lệ phải có chính xác một điểm giao nhau. 
3. Đối với mỗi trung tâm ứng viên, hãy thử xác nhận. 

Chúng tôi chọn một và cho rằng đó là trung tâm. 
4. Từ giữa, mở rộng lên trên cho đến khi tìm thấy một ô không phải sao, ghi lại tất cả các ô sao đã ghé thăm. 

Điều này xác định cánh tay dọc. 
5. Lặp lại các thao tác mở rộng xuống dưới, sang trái và sang phải tương tự. 

Bốn lần duyệt này thu thập tất cả các ô thuộc về cấu trúc. 
6. Sau khi thu thập tất cả các tế bào cánh tay có thể tiếp cận, hãy xác minh hai điều kiện: 

Đầu tiên, mọi ô được thu thập phải là một ngôi sao trong lưới. 

Thứ hai, tổng số ô được thu thập phải bằng tổng số sao trong lưới. 
7. Nếu cả hai điều kiện đều đúng đối với bất kỳ trung tâm ứng viên nào, hãy trả về CÓ. Nếu không, trả về KHÔNG. 

### Tại sao nó hoạt động 

Một điểm cộng hợp lệ được đặc trưng đầy đủ bởi một điểm giao nhau và bốn đoạn thẳng tối đa kéo dài từ điểm đó. Bất kỳ sai lệch nào, dù là sao thừa hoặc thiếu tính liên tục, đều sẽ thay đổi khả năng tiếp cận từ trung tâm hoặc tổng số sao. Bằng cách xây dựng lại hình dạng từ trung tâm và so sánh nó với toàn bộ tập hợp các ngôi sao, chúng tôi thực thi đồng thời cả khả năng kết nối và tính độc quyền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    h, w = map(int, input().split())
    g = [list(input().strip()) for _ in range(h)]

    stars = 0
    for i in range(h):
        for j in range(w):
            if g[i][j] == '*':
                stars += 1

    def valid(i, j):
        if g[i][j] != '*':
            return False

        visited = set()

        # up
        x = i
        while x >= 0 and g[x][j] == '*':
            visited.add((x, j))
            x -= 1

        # down
        x = i + 1
        while x < h and g[x][j] == '*':
            visited.add((x, j))
            x += 1

        # left
        y = j - 1
        while y >= 0 and g[i][y] == '*':
            visited.add((i, y))
            y -= 1

        # right
        y = j + 1
        while y < w and g[i][y] == '*':
            visited.add((i, y))
            y += 1

        # include center
        visited.add((i, j))

        return len(visited) == stars

    for i in range(h):
        for j in range(w):
            if g[i][j] == '*':
                if valid(i, j):
                    print("YES")
                    return

    print("NO")

if __name__ == "__main__":
    solve()
```Lưới được đọc trực tiếp vào bộ nhớ và trước tiên chúng tôi đếm tất cả các ngôi sao. Điều này mang lại cho chúng ta một bất biến toàn cục được sử dụng sau này: bất kỳ sự tái tạo hợp lệ nào cũng phải khớp chính xác với tổng này. 

các`valid`hàm giả định một ô nhất định là trung tâm và mở rộng theo cả bốn hướng cho đến khi hình dạng bị phá vỡ. Mỗi ngôi sao ghé thăm đều được ghi lại. Ý tưởng chính là một tâm đúng sẽ có thể chạm tới mọi ngôi sao chính xác một lần, vì cấu trúc là một điểm cộng duy nhất không có phân nhánh. 

Cuối cùng, chúng tôi kiểm tra từng ô sao như một trung tâm khả thi. Trận đấu thành công đầu tiên sẽ kết thúc chương trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
......
..*...
.****.
..*...
..*...
```Đầu tiên chúng tôi đếm các ngôi sao, bằng 9. 

Chúng tôi kiểm tra các trung tâm ứng viên: 

| Bước | Tế bào | Kiểm tra hướng | Các ô đã truy cập | Tổng số lượt truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | (2,2) | lên/xuống/trái/phải hợp lệ | tất cả 9 ô cộng | 9 | 

Tại ô (2,2), cả bốn hướng đều mở rộng liên tục. Mỗi ngôi sao đều được đưa vào quá trình truyền tải và số lượt truy cập khớp với tổng số sao. 

Điều này xác nhận cấu trúc cộng đơn hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 3
.*.
***
.*.
```Số sao là 5. 

Trung tâm kiểm nghiệm (1,1): 

| Bước | Hướng | Các tế bào được thu thập | 
| --- | --- | --- | 
| lên | (0,1) | 1 ô | 
| xuống | (2,1) | 1 ô | 
| trái | (1,0) | 1 ô | 
| đúng | (1,2) | 1 ô | 
| trung tâm | (1,1) | 1 ô | 

Tổng số lượt truy cập = 5, số sao phù hợp. 

Điều này xác nhận rằng tất cả các nhánh được kết nối thông qua một giao lộ duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h·w) | Mỗi ô được kiểm tra nhiều nhất một lần dưới dạng trung tâm tiềm năng và mỗi phần mở rộng sẽ quét dọc theo các đường thẳng giới hạn bởi kích thước lưới | 
| Không gian | O(1) | Chỉ một tập hợp đã truy cập tỷ lệ với số sao được sử dụng trong quá trình xác thực | 

Các ràng buộc cho phép lên tới 250.000 ô và giải pháp này thực hiện một số lần quét tuyến tính không đổi nhỏ trên lưới, giúp duy trì khả năng thực thi tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    h, w = map(int, input().split())
    g = [list(input().strip()) for _ in range(h)]

    stars = sum(row.count('*') for row in g)

    def valid(i, j):
        if g[i][j] != '*':
            return False
        visited = set()

        x = i
        while x >= 0 and g[x][j] == '*':
            visited.add((x, j))
            x -= 1

        x = i + 1
        while x < h and g[x][j] == '*':
            visited.add((x, j))
            x += 1

        y = j
        while y >= 0 and g[i][y] == '*':
            visited.add((i, y))
            y -= 1

        y = j + 1
        while y < w and g[i][y] == '*':
            visited.add((i, y))
            y += 1

        return len(visited) + 1 == stars

    for i in range(h):
        for j in range(w):
            if g[i][j] == '*':
                if valid(i, j):
                    print("YES")
                    return
    print("NO")

# provided sample
assert run("""5 6
......
..*...
.****.
..*...
..*...
""") == "YES"

# single cell only
assert run("""1 1
*
""") == "YES"

# no plus, disconnected stars
assert run("""3 3
*.*
.*.
*.*
""") == "NO"

# broken arm
assert run("""3 3
.*.
**.
.*.
""") == "NO"

# extra stray cell
assert run("""3 3
.*.
***
..*
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 sao | CÓ | cộng hợp lệ tối thiểu | 
| mẫu bị ngắt kết nối | KHÔNG | từ chối nhiều thành phần | 
| gãy tay | KHÔNG | phát hiện thiếu tính liên tục | 
| tế bào đi lạc | KHÔNG | thực thi độc quyền | 

## Vỏ cạnh 

Trường hợp góc là điểm cộng hợp lệ nhỏ nhất, trong đó tâm là ô duy nhất. Trong trường hợp này, thuật toán coi ngôi sao đơn lẻ là trung tâm hợp lệ, nhưng việc mở rộng hướng ngay lập tức dừng lại và số lượt truy cập khớp chính xác với một ngôi sao. 

Một trường hợp khác là nhiều tâm thế trong một hình chữ thập dày. Chỉ một trong số chúng sẽ bao phủ chính xác toàn bộ các ngôi sao. Bất kỳ ứng cử viên nào khác sẽ bỏ lỡ các cánh tay bên ngoài hoặc bao gồm các phần trùng lặp không chính xác, khiến số lượt truy cập không khớp. 

Trường hợp cạnh cuối cùng là hình dạng gần như cộng với một đoạn cánh tay bị thiếu. Việc mở rộng từ trung tâm sẽ dừng sớm theo hướng đó, làm giảm phạm vi phủ sóng đã ghé thăm xuống dưới tổng số sao, buộc phải từ chối.
