---
title: "CF 106369G - Không quá gần"
description: "Bài toán yêu cầu chúng ta đặt các căn hộ chung cư trên một lưới hình chữ nhật r x c. Một căn hộ chiếm một ô vuông và hai căn hộ không được phép đặt trong các ô vuông tiếp xúc với nhau. Chạm bao gồm tất cả tám hướng: lân cận ngang, dọc và chéo."
date: "2026-06-25T08:20:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106369
codeforces_index: "G"
codeforces_contest_name: "2023 UCF Local Programming Contest"
rating: 0
weight: 106369
solve_time_s: 37
verified: true
draft: false
---

[CF 106369G - Không quá gần](https://codeforces.com/problemset/problem/106369/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề yêu cầu chúng ta đặt các căn hộ chung cư trên một`r`qua`c`lưới hình chữ nhật. Một căn hộ chiếm một ô vuông và hai căn hộ không được phép đặt trong các ô vuông tiếp xúc với nhau. Chạm bao gồm tất cả tám hướng: lân cận ngang, dọc và chéo. 

Nhiệm vụ là đếm xem có bao nhiêu bộ vị trí hợp lệ khác nhau tồn tại. Hai vị trí sẽ khác nhau nếu có ít nhất một hình vuông chứa một căn hộ theo cách sắp xếp này nhưng không phải cách sắp xếp khác. Sự sắp xếp trống cũng hợp lệ. Đáp án có thể rất lớn nên phải in modulo`10^9 + 7`. 

Đầu vào chỉ chứa hai số nguyên, số hàng và số cột của lưới. Số lượng hàng ít, nhiều nhất là 10, trong khi số lượng cột có thể lên tới 1000. Sự bất đối xứng này là manh mối chính. Một giải pháp phụ thuộc theo cấp số nhân vào số hàng là thực tế, nhưng giải pháp phụ thuộc theo cấp số nhân vào số cột là không thể. Vì không thể xử lý lưới có 1000 cột bằng cách thử tất cả các lựa chọn cột, nên chúng tôi cần xử lý lưới từng cột một. 

Một cách hữu ích để suy nghĩ về giới hạn`r <= 10`là một cột chỉ có`2^r`mô hình chiếm chỗ có thể. Đối với mười hàng, đây chỉ là 1024 mặt nạ và sau khi loại bỏ các mặt nạ không hợp lệ ở các căn hộ liền kề theo chiều dọc, con số thậm chí còn nhỏ hơn. Điều này làm cho phương pháp nén trạng thái trở nên khả thi. Một tìm kiếm ngây thơ trên mỗi ô vuông sẽ có`2^(r*c)`những khả năng lớn về mặt thiên văn ngay cả đối với một mạng lưới nhỏ. 

Những trường hợp phức tạp phần lớn là do quên một trong những hướng cấm của hàng xóm. 

Ví dụ: một cột có hai hàng:```
Input:
2 1
```Đầu ra đúng là:```
3
```Các cách sắp xếp có thể là trống, hình vuông trên cùng bị chiếm hoặc hình vuông dưới cùng bị chiếm. Một giải pháp bất cẩn chỉ kiểm tra xung đột theo chiều ngang có thể tính cả hai ô vuông bị chiếm dụng và trả về không chính xác 4. 

Một sai lầm phổ biến khác là bỏ qua các xung đột chéo. Ví dụ:```
Input:
2 2
```Đầu ra đúng là:```
5
```Các sắp xếp hợp lệ không trống duy nhất chứa chính xác một căn hộ, bởi vì mọi cặp hình vuông trong lưới 2 x 2 đều chạm vào nhau. Một giải pháp chỉ ngăn các hàng xóm cạnh nhau và dọc sẽ cho phép các cặp đường chéo không chính xác. 

Lưới nhỏ nhất cũng cần được chú ý:```
Input:
1 1
```Đầu ra đúng là:```
2
```Có hai khả năng: không xây dựng gì hoặc chỉ xây một căn hộ. Việc triển khai khởi tạo mảng lập trình động không chính xác có thể vô tình trả về 0. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét từng ô vuông một cách độc lập và quyết định xem có nên đặt một căn hộ ở đó hay không. Đối với lưới có`r * c`hình vuông, điều này mang lại`2^(r*c)`những sắp xếp có thể. Phương pháp này đúng về mặt logic vì nó kiểm tra mọi tập hợp vị trí có thể có và chỉ giữ lại những vị trí hợp lệ, nhưng nó sẽ không thể sử dụng được ngay lập tức. Ngay cả một lưới chỉ có 10 hàng và 20 cột cũng đã có`2^200`khả năng. 

Lý do vũ lực thất bại không phải là lưới điện không có cấu trúc. Quan sát quan trọng là sự tương tác giữa các cột bị hạn chế. Khi chúng tôi quyết định nội dung của một cột, thông tin duy nhất cần thiết về các cột trước đó là mẫu chiếm chỗ của cột trước đó. Chúng ta không cần phải nhớ toàn bộ lịch sử. 

Một cột có thể được biểu diễn dưới dạng bitmask. Chút`i`là một nếu hàng`i`chứa một căn hộ trong cột đó. Đầu tiên, chúng tôi loại bỏ các mặt nạ có hai bit tập hợp liền kề vì chúng đại diện cho các căn hộ chạm theo chiều dọc trong cùng một cột. 

Sau đó chúng tôi xử lý các cột từ trái sang phải. Đối với mỗi cặp mặt nạ cột liên tiếp, chúng tôi kiểm tra xem chúng có xung đột hay không. Hai cột xung đột nếu một căn hộ xuất hiện trong cùng một hàng, một hàng phía trên hoặc một hàng bên dưới trong cột lân cận. Nếu chúng không xung đột, hai cột có thể xuất hiện liên tiếp theo cách sắp xếp hợp lệ. 

Lực lượng vũ phu hoạt động vì mọi vị trí hoàn chỉnh là một chuỗi các lựa chọn cột. Nó thất bại vì số lượng trình tự rất lớn. Quan sát thấy rằng chỉ các cột liền kề mới ảnh hưởng lẫn nhau cho phép chúng tôi thay thế toàn bộ lịch sử bằng trạng thái lập trình động nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(r*c)) | O(r*c) | Quá chậm | 
| Tối ưu | O(c * S^2) | O(S) | Đã chấp nhận | 

Đây,`S`là số mặt nạ cột hợp lệ. Vì`r = 10`,`S`chỉ là 144. 

#Hướng dẫn thuật toán 

1. Tạo mọi mặt nạ cột có thể từ`0`ĐẾN`2^r - 1`. Chỉ giữ các mặt nạ ở nơi không có hai hàng lân cận nào bị chiếm. Đây là những trạng thái duy nhất có thể có cho một cột vì việc chạm theo chiều dọc bị cấm trong một cột. 
2. Tính toán trước cặp mặt nạ hợp lệ nào có thể được đặt cạnh nhau. Hai mặt nạ tương thích khi chúng không có hàng bị chiếm dụng chung và không có hàng bị chiếm giữ nào bị dịch chuyển bởi một vị trí. Sự thay đổi thể hiện sự tiếp xúc theo đường chéo giữa các cột lân cận. 
3. Khởi tạo giá trị lập trình động của mọi mặt nạ hợp lệ cho cột đầu tiên dưới dạng`1`. Mỗi mặt nạ thể hiện một khả năng sắp xếp cột đầu tiên. 
4. Xử lý từng cột còn lại. Đối với mỗi mặt nạ cột hiện tại, hãy thêm số cách để tiếp cận nó từ mọi mặt nạ tương thích trong cột trước đó. Các cột trước đó không còn quan trọng nữa vì tất cả các xung đột trong tương lai chỉ liên quan đến cột cuối cùng hiện tại. 
5. Tính tổng tất cả các giá trị quy hoạch động sau cột cuối cùng. Mọi trạng thái cột cuối cùng có thể thể hiện một sự sắp xếp lưới hợp lệ hoàn chỉnh. 

Tại sao nó hoạt động: điều bất biến là sau khi xử lý bất kỳ số lượng cột nào,`dp[mask]`bằng số cách sắp xếp hợp lệ của các cột được xử lý trong đó cột cuối cùng có chính xác mẫu chiếm chỗ`mask`. Quá trình chuyển đổi chỉ cho phép các cặp cột không vi phạm bất kỳ quy tắc chạm nào, do đó nó không bao giờ tạo ra sự sắp xếp không hợp lệ. Mọi cách sắp xếp hợp lệ đều có một chuỗi mặt nạ cột duy nhất và mỗi lần chuyển đổi trong trình tự đó đều được xem xét, vì vậy mọi cách sắp xếp hợp lệ đều được tính chính xác một lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    r, c = map(int, input().split())

    masks = []
    for mask in range(1 << r):
        if (mask & (mask << 1)) == 0:
            masks.append(mask)

    n = len(masks)

    transitions = [[] for _ in range(n)]
    for i in range(n):
        a = masks[i]
        for j in range(n):
            b = masks[j]
            if (a & b) == 0 and (a & (b << 1)) == 0 and (a & (b >> 1)) == 0:
                transitions[i].append(j)

    dp = [1] * n

    for _ in range(1, c):
        ndp = [0] * n
        for prev in range(n):
            value = dp[prev]
            if value:
                for nxt in transitions[prev]:
                    ndp[nxt] += value
                    if ndp[nxt] >= MOD:
                        ndp[nxt] -= MOD
        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng danh sách các trạng thái cột pháp lý. Séc`(mask & (mask << 1)) == 0`loại bỏ mặt nạ với các ô vuông bị chiếm đóng theo chiều dọc. Dịch chuyển mặt nạ theo một hàng là đủ vì chỉ các hàng liên tiếp mới có thể chạm theo chiều dọc. 

Cấu trúc chuyển tiếp so sánh từng cặp trạng thái hợp lệ. biểu hiện`(a & b)`bắt các căn hộ trực tiếp cạnh nhau theo chiều ngang. Hai phép so sánh dịch chuyển bắt được các nước láng giềng chéo. Các mặt nạ được lập chỉ mục thay vì được lưu trữ trực tiếp trong mảng lập trình động, giúp quá trình chuyển đổi luôn nhỏ gọn. 

Vòng lặp lập trình động bắt đầu với một cột đã được xử lý. Mỗi trạng thái có một cách tồn tại ở cột đầu tiên, kể cả trạng thái trống. Mỗi cột tiếp theo sẽ tạo một mảng mới vì các giá trị trước đó phải thể hiện chính xác cột trước đó, tránh việc vô tình trộn lẫn các trạng thái từ các lần lặp khác nhau. 

Số nguyên Python không bị tràn, nhưng phép toán modulo vẫn được yêu cầu sau khi cộng vì câu trả lời tăng theo cấp số nhân. Tổng cuối cùng bao gồm mọi trạng thái cột kết thúc có thể có, bao gồm cả cột trống. 

# Ví dụ đã hoạt động 

Không có mẫu chính thức nào trong tuyên bố, vì vậy các dấu vết sau đây sử dụng các trường hợp được chế tạo nhỏ. 

Vì`r = 1`,`c = 3`, lưới là một hàng duy nhất. Các ô liền kề không thể chứa cả các căn hộ, vì vậy câu trả lời tuân theo các lựa chọn độc lập của ba vị trí. 

đầu vào:```
1 3
```| Cột | Trạng thái mặt nạ hiện tại | Giá trị DP | 
| --- | --- | --- | 
| 1 | 0, 1 | 1, 1 | 
| 2 | 0, 1 | 2, 1 | 
| 3 | 0, 1 | 3, 2 | 

Tổng cuối cùng là`3 + 2 = 5`. Những sự sắp xếp này trống, mỗi vị trí đơn lẻ và hai cặp không liền kề. Dấu vết chứng tỏ rằng DP chỉ giữ thông tin cột trước đó trong khi vẫn tính tất cả các khả năng. 

Đối với một lưới có xung đột đường chéo quan trọng: 

đầu vào:```
2 2
```Các mặt nạ hợp lệ là`00`,`01`, Và`10`. 

| Cột | Trạng thái trước đó | Tiểu bang mới | Giá trị gia tăng | 
| --- | --- | --- | --- | 
| 1 | bắt đầu | 00 | 1 | 
| 1 | bắt đầu | 01 | 1 | 
| 1 | bắt đầu | 10 | 1 | 
| 2 | 00 | 00 | 1 | 
| 2 | 00 | 01 | 1 | 
| 2 | 00 | 10 | 1 | 
| 2 | 01 | 00 | 1 | 
| 2 | 10 | 00 | 1 | 

Số đếm cuối cùng là`5`: trống hoặc một trong bốn ô. Quá trình chuyển đổi sẽ loại bỏ chính xác tất cả các cặp mặt nạ không trống vì mọi cặp ô trong lưới 2 x 2 đều chạm vào. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(c * S^2) | Mỗi lần chuyển đổi cột sẽ kiểm tra các cặp mặt nạ hợp lệ tương thích. | 
| Không gian | O(S^2) | Danh sách chuyển tiếp cộng với hai mảng lập trình động được lưu trữ. | 

Vì`r <= 10`, số lượng mặt nạ hợp lệ rất ít nên thậm chí 1000 cột cũng có thể được xử lý dễ dàng. Thuật toán tránh sự phụ thuộc vào tổng số ô và phù hợp thoải mái trong giới hạn dự định. 

# Trường hợp thử nghiệm```python
import sys, io

MOD = 10 ** 9 + 7

def solution(inp):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    r, c = map(int, sys.stdin.readline().split())

    masks = []
    for mask in range(1 << r):
        if (mask & (mask << 1)) == 0:
            masks.append(mask)

    n = len(masks)
    trans = [[] for _ in range(n)]

    for i, a in enumerate(masks):
        for j, b in enumerate(masks):
            if (a & b) == 0 and (a & (b << 1)) == 0 and (a & (b >> 1)) == 0:
                trans[i].append(j)

    dp = [1] * n
    for _ in range(c - 1):
        ndp = [0] * n
        for i, val in enumerate(dp):
            for j in trans[i]:
                ndp[j] = (ndp[j] + val) % MOD
        dp = ndp

    ans = str(sum(dp) % MOD)
    sys.stdin = old_stdin
    return ans

assert solution("1 1\n") == "2", "minimum grid"
assert solution("2 2\n") == "5", "all cells touch in a 2x2 grid"
assert solution("1 5\n") == "13", "single row Fibonacci case"
assert solution("2 1\n") == "3", "vertical boundary case"
assert solution("3 3\n") == "35", "small multi-row grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Lưới nhỏ nhất có thể và xử lý vị trí trống | 
|`2 2`|`5`| Xử lý xung đột theo đường chéo | 
|`1 5`|`13`| Giảm một chiều | 
|`2 1`|`3`| Ranh giới liền kề dọc | 
|`3 3`|`35`| Hồ sơ chung Hành vi DP | 

# Vỏ cạnh 

Đối với đầu vào:```
2 1
```mặt nạ được tạo ra là`00`,`01`, Và`10`. Vì chỉ có một cột nên không có sự chuyển tiếp theo chiều ngang nào cần xem xét. Câu trả lời là số lượng mặt nạ một cột hợp lệ là 3. Thuật toán xử lý việc này vì mảng lập trình động ban đầu đã thể hiện toàn bộ lưới khi`c = 1`. 

Đối với đầu vào:```
2 2
```thuật toán tạo ra ba trạng thái cột giống nhau. Khi kiểm tra quá trình chuyển đổi, mọi cặp trạng thái không trống đều bị từ chối do các hàng chồng lên nhau hoặc mặt nạ dịch chuyển chồng lên nhau theo đường chéo. Chỉ còn lại các chuyển đổi liên quan đến trạng thái trống, đưa ra tổng cộng năm cách sắp xếp. Điều này nắm bắt các triển khai quên kiểm tra đường chéo. 

Đối với đầu vào:```
1 5
```các bang duy nhất trống rỗng và bị chiếm đóng. Quá trình chuyển đổi chỉ cho phép trạng thái bị chiếm đóng sau trạng thái trống, vì hai cột liền kề không thể chứa cả căn hộ. DP tạo ra chuỗi các bộ độc lập trên một dòng, kết thúc bằng 13 cách sắp xếp. 

Đối với đầu vào:```
1 1
```mảng trạng thái ban đầu chứa hai khả năng. Vì không cần chuyển tiếp nên câu trả lời cuối cùng chỉ đơn giản là tổng của các trạng thái ban đầu. Điều này xác nhận rằng việc sắp xếp căn hộ trống và việc sắp xếp căn hộ chung cư đơn lẻ đều được tính.
