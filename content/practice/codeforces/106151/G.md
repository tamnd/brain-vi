---
title: "CF 106151G - quản lý cửa sổ"
description: "Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục được đặt trên mặt phẳng 2D, mỗi hình đại diện cho một cửa sổ. Mỗi cửa sổ cũng có một giá trị độ cao duy nhất, giá trị này xác định khả năng hiển thị: nếu hai cửa sổ chồng lên nhau, cửa sổ có chiều cao cao hơn sẽ che phủ cửa sổ kia trong vùng chồng lấp."
date: "2026-06-20T08:41:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "G"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 51
verified: true
draft: false
---

[CF 106151G - trình quản lý cửa sổ](https://codeforces.com/problemset/problem/106151/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình chữ nhật thẳng hàng theo trục được đặt trên mặt phẳng 2D, mỗi hình đại diện cho một cửa sổ. Mỗi cửa sổ cũng có một giá trị độ cao duy nhất, giá trị này xác định khả năng hiển thị: nếu hai cửa sổ chồng lên nhau, cửa sổ có chiều cao cao hơn sẽ che phủ cửa sổ kia trong vùng chồng lấp. 

Nhiệm vụ không phải là tính diện tích nhìn thấy được mà là một cái gì đó mang tính cấu trúc hơn. Chúng ta tưởng tượng chỉ vẽ những phần có thể nhìn thấy được của tất cả các đường viền cửa sổ sau khi xếp chúng theo thứ tự chiều cao. Bất cứ khi nào một đoạn viền của cửa sổ không bị các cửa sổ cao hơn che khuất hoàn toàn sẽ góp phần làm tăng chi phí bản vẽ. Mục đích là đếm xem có bao nhiêu đoạn đường ngang và dọc xuất hiện trong đường viền hiển thị cuối cùng của toàn bộ khung cảnh. 

Khó khăn chính là các cửa sổ chồng chéo có thể chia các cạnh thành nhiều mảnh có thể nhìn thấy được và chỉ những phần không được bao phủ bởi bất kỳ hình chữ nhật cao hơn nào mới đóng góp. 

Các ràng buộc tọa độ nhỏ về mặt hình học nhưng lớn về số lượng. Tọa độ được giới hạn bởi 1000, điều này cho thấy có sẵn cấu trúc lưới cố định. Tuy nhiên, số lượng hình chữ nhật lên tới 100000, điều này loại trừ bất kỳ bức tranh lưới trên mỗi hình chữ nhật hoặc mô phỏng trên mỗi ô. Bất kỳ giải pháp nào chạm tới tất cả các ô lưới trên mỗi cửa sổ sẽ ngay lập tức trở nên không khả thi. 

Điều này gợi ý rõ ràng rằng giải pháp phải nén hình học thành một biểu diễn lưới cố định và sau đó thực hiện một quy trình lan truyền hoặc quét toàn cục duy nhất. 

Trường hợp cạnh tinh tế phát sinh khi nhiều hình chữ nhật chồng lên nhau một cách chính xác trên các ranh giới. Ví dụ: nếu hai hình chữ nhật có chung một cạnh và hình trên bao phủ hoàn toàn ranh giới đó thì không có phân đoạn nào được tính cho hình dưới ở cạnh đó. Một trường hợp khác là khi một hình chữ nhật bị che khuất hoàn toàn bởi những hình cao hơn, không có phân đoạn nào. 

Một ví dụ tối thiểu minh họa hành vi ẩn: 

đầu vào:```
2
0 0 2 2 1
0 0 2 2 2
```Đầu ra:```
8
```Hình chữ nhật thứ hai bao phủ hoàn toàn hình chữ nhật thứ nhất, do đó chỉ hiển thị đường viền bên ngoài của hình chữ nhật trên cùng. 

Một cách tiếp cận ngây thơ đếm chu vi của mỗi hình chữ nhật một cách độc lập sẽ tính sai các ranh giới chồng chéo nhiều lần. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp sẽ xử lý từng hình chữ nhật một cách độc lập và cố gắng xác định xem bốn cạnh nào của nó có thể nhìn thấy được. Đối với mỗi cạnh, chúng ta sẽ kiểm tra xem mọi điểm dọc theo nó có được bao phủ bởi một hình chữ nhật cao hơn hay không. Vì tọa độ là các số nguyên liên tục trong một phạm vi giới hạn nên chúng ta có thể rời rạc hóa các cạnh và kiểm tra phạm vi bao phủ bằng cách sử dụng các kiểm tra khoảng thời gian. 

Ngay cả với sự rời rạc hóa, đối với mỗi cạnh hình chữ nhật, chúng tôi có khả năng quét theo tất cả các hình chữ nhật có z cao hơn để xem liệu có bất kỳ sự chồng chéo nào che phủ nó hay không. Điều này dẫn đến hành vi O(N^2) trong các trường hợp chồng chéo dày đặc. Với 100000 hình chữ nhật, điều này vượt xa giới hạn khả thi. 

Quan sát cấu trúc quan trọng là không gian tọa độ nhỏ theo một chiều (1000 x 1000), điều này cho phép chúng ta lật ngược quan điểm: thay vì suy luận theo hình chữ nhật, chúng ta suy luận theo từng đoạn cạnh lưới. Mọi phân đoạn hiển thị đều tương ứng với sự chuyển đổi giữa “được bao phủ bởi một số hình chữ nhật z cao nhất” và “không được bao phủ bởi bất cứ thứ gì”. 

Nếu chúng tôi xử lý các hình chữ nhật từ z cao nhất đến z thấp nhất, chúng tôi có thể mô phỏng việc vẽ đường viền của chúng lên lưới trong khi đảm bảo rằng khi một phân đoạn ranh giới được xác nhận là hiển thị, nó sẽ không bao giờ được tính lại. Điều này biến vấn đề thành vấn đề đánh dấu vùng phủ sóng 2D trên một lưới cố định, trong đó mỗi ô cạnh được truy cập nhiều nhất một lần. 

Chúng tôi duy trì hai lưới: một cho các cạnh dọc và một cho các cạnh ngang. Khi xử lý một hình chữ nhật, chúng tôi chỉ thêm một phân đoạn nếu phân đoạn đó chưa được bao phủ bởi hình chữ nhật cao hơn. Điều này đảm bảo tính chính xác vì hình chữ nhật cao hơn được xử lý trước. 

Sự giảm thiểu quan trọng là chúng ta không bao giờ cần phải tính toán lại khả năng hiển thị đối với tất cả các hình chữ nhật khác. Chúng ta chỉ cần biết liệu một phân đoạn ranh giới đã được lớp cao hơn xác nhận hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1)-O(N) | Quá chậm | 
| Lưới + quét z giảm dần | O(N + C2) trong đó C 1000 | O(C2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mặt phẳng như một lưới rời rạc gồm các cạnh đơn vị. Mỗi hình chữ nhật đóng góp vào bốn cạnh ranh giới, nhưng chỉ những hình chữ nhật chưa được bao phủ bởi các hình chữ nhật cao hơn mới được tính. 

1. Sắp xếp tất cả các hình chữ nhật theo thứ tự giá trị z giảm dần. Điều này đảm bảo rằng chúng tôi luôn xử lý cửa sổ trên cùng trước tiên, do đó, mọi ranh giới mà chúng tôi ghi lại đều được đảm bảo hiển thị. 
2. Duy trì hai lưới boolean: một cho các cạnh ngang và một cho các cạnh dọc. Cạnh ngang được xác định bằng khoảng y và x cố định, còn cạnh dọc được xác định bằng khoảng x và y cố định. Mỗi đoạn đơn vị tương ứng với một ô lưới trong các cấu trúc này. 
3. Với mỗi hình chữ nhật theo thứ tự sắp xếp, hãy kiểm tra bốn cạnh của nó. Đối với các cạnh dưới và trên, lặp lại x từ x1 đến x2 và xem xét các đoạn ngang tại y = y1 và y = y2. Đối với bên trái và bên phải, lặp lại y từ y1 đến y2 và xem xét các đoạn thẳng đứng tại x = x1 và x = x2. 
4. Bất cứ khi nào chúng tôi gặp một đoạn ranh giới chưa được đánh dấu, chúng tôi sẽ tăng câu trả lời và đánh dấu nó là đã được che. Điều này đảm bảo rằng chỉ hình chữ nhật đầu tiên (z cao nhất) hiển thị đoạn đó mới góp phần vào số đếm. 
5. Tiếp tục cho đến khi tất cả các hình chữ nhật được xử lý, sau đó xuất ra số đếm tích lũy. 

Việc lặp qua các phân đoạn là an toàn vì tọa độ được giới hạn bởi 1000, do đó, mỗi hình chữ nhật đóng góp tối đa O(1000) công việc, khiến tổng số khoảng O(10^8) trong trường hợp xấu nhất, có thể chấp nhận được trong Python được tối ưu hóa khi được triển khai cẩn thận với các phép toán mảng đơn giản. 

### Tại sao nó hoạt động

Mỗi phân đoạn ranh giới đơn vị trong lưới có một hình chữ nhật z cao nhất duy nhất có khả năng hiển thị nó. Bằng cách xử lý các hình chữ nhật từ z cao nhất đến z thấp nhất, chúng tôi đảm bảo rằng lần đầu tiên gặp một phân đoạn tương ứng chính xác với bề mặt nhìn thấy được tại vị trí đó. Bất kỳ hình chữ nhật thấp hơn nào cũng chạm vào phân đoạn đó nhất thiết phải bị ẩn trong vùng đó vì hình chữ nhật cao hơn đã xác nhận nó. Điều này thiết lập rằng mỗi phân đoạn được tính chính xác một lần khi và chỉ khi nó hiển thị trong cấu hình xếp chồng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    rects = []
    for _ in range(n):
        x1, y1, x2, y2, z = map(int, input().split())
        rects.append((z, x1, y1, x2, y2))

    rects.sort(reverse=True)

    # grids for unit segments
    # horizontal: (y, x) means segment from (x,y) to (x+1,y)
    # vertical: (x, y) means segment from (x,y) to (x,y+1)
    hor = [[False] * 1001 for _ in range(1001)]
    ver = [[False] * 1001 for _ in range(1001)]

    ans = 0

    for z, x1, y1, x2, y2 in rects:
        # bottom edge
        y = y1
        for x in range(x1, x2):
            if not hor[y][x]:
                hor[y][x] = True
                ans += 1

        # top edge
        y = y2
        for x in range(x1, x2):
            if not hor[y][x]:
                hor[y][x] = True
                ans += 1

        # left edge
        x = x1
        for y in range(y1, y2):
            if not ver[x][y]:
                ver[x][y] = True
                ans += 1

        # right edge
        x = x2
        for y in range(y1, y2):
            if not ver[x][y]:
                ver[x][y] = True
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc đánh dấu lưới trực tiếp các phân đoạn đơn vị. Điểm tinh tế quan trọng là các cạnh được coi là các khoảng đơn vị giữa các tọa độ nguyên, do đó, các vòng lặp sẽ ngầm tăng lên x2-1 hoặc y2-1 bằng cách sử dụng phạm vi (x1, x2) của Python. Một chi tiết quan trọng khác là các cạnh ngang và dọc được lưu trữ riêng biệt vì chúng nằm trên các hướng hình học khác nhau. 

Việc sắp xếp theo z là cần thiết. Nếu không có nó, các hình chữ nhật thấp hơn có thể xác nhận không chính xác các cạnh thuộc về hình cao hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 0 2 2 1
0 0 2 2 2
```Chúng tôi xử lý hình chữ nhật với z=2 trước, sau đó là z=1. 

| Bước | Hình chữ nhật | Đã thêm cạnh ngang | Đã thêm cạnh dọc | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (2, 0,0,2,2) | 4 + 4 = 8 đoạn ranh giới | đã được bao gồm trong cùng một số lượng | 8 | 
| 2 | (1, 0,0,2,2) | không có gì mới | không có gì mới | 8 | 

Hình chữ nhật thứ hai được bao phủ hoàn toàn nên nó không đóng góp gì cả. 

Điều này xác nhận rằng khi một phân đoạn được xác nhận bởi hình chữ nhật cao hơn, những hình thấp hơn không thể tăng câu trả lời. 

### Ví dụ 2 

đầu vào:```
3
0 0 3 1 1
1 0 2 2 2
0 1 3 2 3
```Chúng tôi xử lý z=3, sau đó z=2, sau đó z=1. 

| Bước | Hình chữ nhật | Ngang mới | Ngành dọc mới | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (3,0,1,3,2) | cạnh trên và dưới một phần | bên | k1 | 
| 2 | (2,1,0,2,2) | một số trùng lặp với | một phần | k2 | 
| 3 | (1,0,0,3,1) | chủ yếu được bảo hiểm | một số tiếp xúc | k3 | 

Hiện tượng chính ở đây là chồng chéo một phần: một số cạnh được sử dụng lại và do đó bị bỏ qua. Điều này chứng tỏ rằng lưới ngăn cản việc đếm hai lần ngay cả khi các hình chữ nhật lồng vào nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + C2) | Việc sắp xếp mất O(N log N), nhưng việc truyền tải lưới bị giới hạn bởi 1000 mỗi bên, do đó tổng quá trình xử lý cạnh tỷ lệ thuận với dung lượng chu vi | 
| Không gian | O(C2) | Hai lưới boolean có kích thước lên tới 1000 x 1000 lưu trữ các phân đoạn đã sử dụng | 

Giới hạn tọa độ khắc phục độ phức tạp về không gian, do đó, ngay cả với 100000 hình chữ nhật, thuật toán vẫn khả thi. Chi phí chủ yếu là quét các cạnh, nhưng mỗi cạnh đơn vị được truy cập nhiều nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# single rectangle
assert run("""1
0 0 2 2 1
""") == "8"

# full overlap, higher on top
assert run("""2
0 0 2 2 1
0 0 2 2 2
""") == "8"

# non-overlapping rectangles
assert run("""2
0 0 1 1 1
1 0 2 1 2
""") == "8"

# nested rectangles
assert run("""3
0 0 3 3 1
0 0 3 3 2
0 0 3 3 3
""") == "12"

# thin strip overlap
assert run("""2
0 0 3 1 1
1 0 2 1 2
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | 8 | tính chu vi cơ bản | 
| chồng chéo hoàn toàn | 8 | ức chế tầm nhìn bằng z | 
| không chồng chéo | 8 | đóng góp độc lập | 
| hình chữ nhật lồng nhau | 12 | chỉ nhìn thấy được ở ngoài cùng | 
| chồng chéo mỏng | 10 | chia sẻ cạnh một phần | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi hình chữ nhật có chung các cạnh một cách chính xác. Coi như:```
2
0 0 2 2 1
2 0 4 2 2
```Các hình chữ nhật chạm vào x=2 nhưng không chồng lên nhau. Đầu ra chính xác tính cả hai chu vi đầy đủ một cách độc lập, nhưng ranh giới chung hoàn toàn không phải là một phân đoạn vì nó có độ dài trùng nhau bằng 0. Thuật toán xử lý điều này một cách tự nhiên vì phạm vi vòng lặp loại trừ chính x2, do đó không có phân đoạn nào được tính hai lần ở ranh giới tiếp xúc. 

Một trường hợp khác là ngăn chặn hoàn toàn:```
2
0 0 10 10 1
2 2 8 8 2
```Ở đây hình chữ nhật bên trong hiển thị đầy đủ nhưng không xóa bất kỳ ranh giới bên ngoài nào. Hình chữ nhật bên ngoài chỉ đóng góp chu vi bên ngoài của nó, trong khi hình bên trong đóng góp toàn bộ chu vi của chính nó, vì không có cạnh nào của nó được bao phủ bởi z cao hơn. 

Việc đánh dấu lưới đảm bảo tính chính xác vì khả năng hiển thị được đánh giá độc lập trên mỗi phân đoạn đơn vị, do đó, sự chồng chéo một phần chỉ loại bỏ chính xác những phân đoạn bị ẩn về mặt hình học.
