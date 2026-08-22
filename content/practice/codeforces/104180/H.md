---
title: "CF 104180H - Bức tranh không đẹp lắm"
description: "Chúng ta được cung cấp một khung vẽ hình chữ nhật rất lớn, về mặt khái niệm là một lưới có tọa độ lên tới $10^9 nhân 10^9$. Trên khung vẽ này, Bob đã vẽ một số hình chữ nhật thẳng hàng với trục."
date: "2026-07-02T00:44:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 63
verified: true
draft: false
---

[CF 104180H - Bức tranh không đẹp lắm](https://codeforces.com/problemset/problem/104180/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một khung vẽ hình chữ nhật rất lớn, về mặt khái niệm là một lưới có tọa độ lên tới$10^9 \times 10^9$. Trên khung vẽ này, Bob đã vẽ một số hình chữ nhật thẳng hàng với trục. Các hình chữ nhật này không chồng lên nhau, vì vậy mỗi ô được tô thuộc nhiều nhất một hình chữ nhật. 

Sau khi vẽ xong, một dãy cột thẳng đứng bị mưa cuốn trôi. Mỗi cột được rửa sẽ loại bỏ mọi ô được sơn có chỉ số cột khớp với một trong các giá trị đã cho. Nhiệm vụ là tính xem còn lại bao nhiêu ô được sơn sau khi xóa tất cả các ô được sơn nằm trong các cột đã được xóa đó. 

Đầu vào bao gồm các hình chữ nhật không chồng chéo được mô tả bởi các góc dưới cùng bên trái và trên cùng bên phải của chúng và một danh sách các chỉ mục cột bị hủy. Đầu ra là tổng diện tích sơn còn lại sau khi loại bỏ tất cả các ô được sơn trong các cột đó. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$hình chữ nhật và$2 \cdot 10^5$các cột được rửa sạch, trong khi tọa độ có thể lớn bằng$10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng dựa trên lưới. Ngay cả việc lưu trữ thông tin trên mỗi ô hoặc mỗi cột một cách rõ ràng cũng là không thể. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề thành các phép toán theo khoảng thời gian hoặc số lượng tổng hợp, lý tưởng nhất là trong$O((N + M) \log N)$hoặc$O(N + M)$. 

Một điểm tinh tế là hình chữ nhật có tính rời rạc. Điều này loại bỏ nhu cầu nén tọa độ trên các hình dạng chồng chéo và đảm bảo rằng phần đóng góp của mỗi hình chữ nhật có thể được tính tổng một cách độc lập và an toàn. 

Các trường hợp cạnh phát sinh từ cách các cột tương tác với hình chữ nhật: 

Một hình chữ nhật có chiều rộng bằng 0 trong thực tế có thể xảy ra nếu$c_1 = c_2$, nghĩa là một dải cột đơn. Nếu cột đó bị xóa, toàn bộ hình chữ nhật sẽ biến mất. 

Ví dụ: 

đầu vào:```
1 1
3 1 3 5
3
```Đầu ra:```
0
```Một cách giải thích đơn giản trên mỗi ô vẫn có thể tính không chính xác nếu nó giả sử chiều rộng dương mà không xem xét các hình chữ nhật suy biến. 

Một trường hợp cạnh khác là khi không có cột được rửa nào giao nhau với bất kỳ hình chữ nhật nào. Câu trả lời phải bằng tổng diện tích hình chữ nhật. 

Ví dụ:```
1 2
1 1 3 3
10
20
```Đầu ra:```
9
```Cuối cùng, nếu nhiều hình chữ nhật chồng lên cùng một phạm vi cột nhưng rời rạc về không gian, chúng ta phải tránh tính hai lần, điều này được đảm bảo bởi tuyên bố vấn đề nhưng dễ vô tình đưa lại nếu quá trình xử lý được thực hiện trên mỗi cột mà không cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mở rộng mọi hình chữ nhật thành các ô đơn vị và trừ đi các cột đã được xóa. Điều này ngay lập tức không thể thực hiện được. Một hình chữ nhật có thể che tối đa$10^{18}$các ô trong trường hợp xấu nhất và thậm chí việc lặp qua các cột trên mỗi hình chữ nhật sẽ giảm xuống$O(N \cdot \text{width})$. 

Một quan sát đầu tiên tốt hơn là việc rửa chỉ ảnh hưởng đến cột chứ không ảnh hưởng đến hàng. Bên trong một hình chữ nhật nhất định, mỗi cột đóng góp chính xác chiều cao của nó vào tổng diện tích sơn. Vì vậy, mỗi hình chữ nhật có thể được coi là một phần đóng góp của chiều cao thẳng đứng được áp dụng thống nhất trên khoảng cột của nó$[c_1, c_2]$. Vấn đề trở thành: đối với mỗi hình chữ nhật, hãy tính xem có bao nhiêu cột của nó không nằm trong tập hợp đã loại bỏ và nhân với chiều cao của nó. 

Khó khăn còn lại là tập cột bị loại bỏ lớn và tùy ý. Kiểm tra tư cách thành viên trên mỗi cột quá chậm. Thay vào đó, chúng tôi sắp xếp các cột đã bị xóa và sử dụng tìm kiếm nhị phân hoặc quét con trỏ để đếm các giao điểm giữa khoảng hình chữ nhật và tập hợp đã xóa. 

Vì hình chữ nhật không chồng lên nhau nên chúng ta có thể xử lý từng hình một cách độc lập. Đối với mỗi hình chữ nhật, chúng tôi đếm có bao nhiêu cột bị xóa nằm trong phạm vi ngang của nó bằng cách sử dụng tìm kiếm nhị phân trên một mảng được sắp xếp. Trừ đi giá trị này từ tổng chiều rộng sẽ cho các cột còn sót lại. 

Điều này làm giảm vấn đề sắp xếp$M$cột và trả lời$N$truy vấn đếm phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng ô/cột) |$O(N \cdot W)$|$O(W)$| Quá chậm | 
| Tối ưu (sắp xếp + tìm kiếm nhị phân trên mỗi hình chữ nhật) |$O((N+M)\log M)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán sang việc đếm xem còn lại bao nhiêu cột “tốt” bên trong mỗi hình chữ nhật. 

1. Đọc tất cả các chỉ số cột đã rửa và sắp xếp chúng. Việc sắp xếp là cần thiết để chúng ta có thể đếm một cách hiệu quả số lượng nằm trong bất kỳ khoảng nào bằng cách sử dụng tìm kiếm nhị phân. Nếu không sắp xếp, mỗi truy vấn sẽ yêu cầu quét tất cả các cột. 
2. Khởi tạo câu trả lời cuối cùng về 0. Chúng tôi sẽ tích lũy các khoản đóng góp từ mỗi hình chữ nhật một cách độc lập vì chúng không trùng nhau. 
3. Với mỗi hình chữ nhật, hãy tính chiều cao thẳng đứng của nó là$r_2 - r_1 + 1$. Điều này thể hiện mỗi cột hợp lệ đóng góp bao nhiêu trong hình chữ nhật đó. 
4. Xác định tổng số cột trong nhịp ngang của hình chữ nhật là$c_2 - c_1 + 1$. 
5. Sử dụng tìm kiếm nhị phân để đếm xem có bao nhiêu cột được rửa sạch$[c_1, c_2]$. Điều này được thực hiện bằng cách tìm chỉ mục đầu tiên$\ge c_1$và chỉ số đầu tiên$> c_2$, và trừ hai vị trí. 
6. Trừ các cột đã rửa khỏi tổng số cột để thu được các cột còn sót lại bên trong hình chữ nhật. 
7. Nhân các cột còn lại với chiều cao hình chữ nhật và cộng vào đáp án. 

Mỗi hình chữ nhật được xử lý độc lập nên chúng tôi không bao giờ gặp rủi ro khi tính hai lần hoặc gây nhiễu giữa các khu vực. 

### Tại sao nó hoạt động 

Đặc tính cấu trúc quan trọng là các hình chữ nhật không chồng lên nhau, điều này làm cho diện tích trở nên cộng thêm. Mỗi hình chữ nhật đóng góp một khối ô hình chữ nhật và quá trình rửa hoạt động độc lập trên các cột, loại bỏ toàn bộ các lát dọc một cách đồng đều trên tất cả các hình chữ nhật. Điều này có nghĩa là sự tồn tại của một ô chỉ phụ thuộc vào chỉ số cột của nó chứ không phụ thuộc vào hình chữ nhật mà nó thuộc về. Kết quả là, mỗi hình chữ nhật có thể được đánh giá bằng cách chiếu tập hợp toàn bộ các cột đã bị loại bỏ lên khoảng của nó và chia tỷ lệ theo chiều cao. Bước tìm kiếm nhị phân tính toán chính xác kích thước hình chiếu này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    rects = []
    for _ in range(n):
        r1, c1, r2, c2 = map(int, input().split())
        rects.append((r1, c1, r2, c2))
    
    washed = [int(input()) for _ in range(m)]
    washed.sort()
    
    import bisect
    
    ans = 0
    
    for r1, c1, r2, c2 in rects:
        height = r2 - r1 + 1
        total_cols = c2 - c1 + 1
        
        left = bisect.bisect_left(washed, c1)
        right = bisect.bisect_right(washed, c2)
        
        bad_cols = right - left
        good_cols = total_cols - bad_cols
        
        if good_cols > 0:
            ans += good_cols * height
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp tất cả các cột đã được rửa để chúng ta có thể truy vấn các phạm vi một cách hiệu quả. Đối với mỗi hình chữ nhật, chúng tôi tính toán đóng góp hình học của nó về chiều cao và chiều rộng. Các phép toán chia đôi sẽ xác định chính xác có bao nhiêu cột được rửa rơi vào khoảng ngang của hình chữ nhật. Trừ đi những giá trị này sẽ cho số lượng cột còn sót lại và nhân với chiều cao sẽ chuyển đổi số lượng cột thành diện tích. 

Một lỗi phổ biến là quên rằng mỗi cột đóng góp toàn bộ chiều cao chứ không chỉ một ô. Một nguyên nhân khác là tính toán sai các ranh giới khoảng, đặc biệt khi cả hai điểm cuối đều bao hàm. sử dụng`bisect_left`Và`bisect_right`đảm bảo tính toán phạm vi một cách chính xác mà không cần điều chỉnh từng cái một theo cách thủ công. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
3 1 5 4
1 5 4 6
1 2 1 3
5 5 5 5
2
4
5
```Đã sắp xếp các cột đã rửa: [2, 4, 5] 

Chúng tôi xử lý từng hình chữ nhật: 

| Hình chữ nhật | Chiều cao | Phạm vi cột | Tổng số tiền | đối tác xấu | bạn tốt | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| (3,1,5,4) | 3 | [1,4] | 4 | 2 (2,4) | 2 | 6 | 
| (1,5,4,6) | 4 | [5,6] | 2 | 1 (5) | 1 | 4 | 
| (1,2,1,3) | 1 | [2,3] | 2 | 1 (2) | 1 | 1 | 
| (5,5,5,5) | 1 | [5,5] | 1 | 1 (5) | 0 | 0 | 

Câu trả lời cuối cùng là 11. 

Dấu vết này cho thấy mỗi hình chữ nhật được xử lý độc lập như thế nào và các cột được rửa như thế nào chỉ có liên quan thông qua số lượng giao lộ. 

### Mẫu 2 

đầu vào:```
3 3
1 5 3 7
1 8 2 8
4 6 5 8
1
3
10
```Đã sắp xếp các cột đã rửa: [1, 3, 10] 

| Hình chữ nhật | Chiều cao | Phạm vi cột | Tổng số tiền | đối tác xấu | bạn tốt | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| (1,5,3,7) | 3 | [5,7] | 3 | 0 | 3 | 9 | 
| (1,8,2,8) | 2 | [8,8] | 1 | 0 | 1 | 2 | 
| (4,6,5,8) | 2 | [6,8] | 3 | 0 | 3 | 6 | 

Tổng cộng là 17. 

Ví dụ này xác nhận rằng các cột được rửa bên ngoài tất cả các phạm vi hình chữ nhật không ảnh hưởng đến kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + M)\log M)$| Sắp xếp các cột bị rửa chiếm ưu thế$O(M \log M)$và mỗi truy vấn hình chữ nhật sử dụng hai tìm kiếm nhị phân | 
| Không gian |$O(M)$| Lưu trữ danh sách các cột đã được sắp xếp | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử và truy vấn logarit trên phạm vi này phù hợp thoải mái trong giới hạn thời gian. Giải pháp tránh mọi sự phụ thuộc vào kích thước tọa độ, chỉ dựa vào xử lý sự kiện được sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    rects = []
    for _ in range(n):
        rects.append(tuple(map(int, input().split())))
    washed = [int(input()) for _ in range(m)]
    washed.sort()
    import bisect

    ans = 0
    for r1, c1, r2, c2 in rects:
        h = r2 - r1 + 1
        total = c2 - c1 + 1
        l = bisect.bisect_left(washed, c1)
        r = bisect.bisect_right(washed, c2)
        good = total - (r - l)
        ans += good * h
    return str(ans)

# provided samples
assert run("""4 3
3 1 5 4
1 5 4 6
1 2 1 3
5 5 5 5
2
4
5
""") == "11"

assert run("""3 3
1 5 3 7
1 8 2 8
4 6 5 8
1
3
10
""") == "17"

# minimum-size: single cell unaffected
assert run("""1 1
1 1 1 1
2
""") == "1"

# fully washed single rectangle column
assert run("""1 1
1 5 2 5
5
""") == "0"

# all columns safe
assert run("""2 0
1 1 2 2
3 3 4 4
""") == "8"

# boundary overlap check
assert run("""1 3
1 1 3 3
1
2
3
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| két sắt đơn | 1 | trường hợp tối thiểu | 
| cột đơn bị xóa | 0 | lau toàn bộ hình chữ nhật mỏng | 
| không có cột được rửa | toàn bộ khu vực | hành vi nhận dạng | 
| rửa chồng chéo đầy đủ | 0 | độ chính xác bao gồm ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một hình chữ nhật thoái hóa thành một cột duy nhất. Ví dụ: 

đầu vào:```
1 1
2 7 5 7
7
```Hình chữ nhật có chiều cao 4 và chiều rộng 1. Cột được rửa hoàn toàn khớp với nó. Tìm kiếm nhị phân tìm thấy một cột xấu bên trong$[7,7]$, làm cho các cột tốt trở thành số 0 và đóng góp trở thành số không. Điều này xác nhận tính chính xác cho các khoảng chiều rộng một. 

Một trường hợp cạnh khác xảy ra khi các cột được rửa nằm hoàn toàn bên ngoài tất cả các hình chữ nhật: 

đầu vào:```
1 2
1 1 3 3
10
20
```Các tìm kiếm nhị phân trả về 0 cột xấu cho khoảng hình chữ nhật$[1,3]$, do đó toàn bộ diện tích được bảo toàn. Thuật toán xử lý việc này một cách tự nhiên mà không cần cách viết hoa đặc biệt. 

Trường hợp tinh vi cuối cùng là nhiều hình chữ nhật có khoảng cách ngang rời nhau nhưng có chung các cột được chia đều. Vì mỗi hình chữ nhật truy vấn độc lập cùng một mảng được sắp xếp nên không có sự can thiệp hoặc tính hai lần và mỗi đóng góp được tính toán nghiêm ngặt trong giới hạn riêng của nó.
