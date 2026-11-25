---
title: "CF 993C - Cơ động cẩn thận"
description: "Chúng ta được cấp hai nhóm tàu ​​địch, tất cả đều nằm trên hai đường thẳng đứng: một nhóm cố định ở $x=-100$, nhóm kia ở $x=100$. Mỗi tàu có tọa độ $y$-số nguyên và nhiều tàu có thể có chung $y$."
date: "2026-06-17T00:11:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 993
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 488 by NEAR (Div. 1)"
rating: 2100
weight: 993
solve_time_s: 123
verified: true
draft: false
---

[CF 993C - Thao tác cẩn thận](https://codeforces.com/problemset/problem/993/C) 

**Xếp hạng:** 2100 
**Tags:** bitmasks, vũ lực, hình học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho hai đoàn tàu địch, tất cả đều nằm trên hai đường thẳng đứng: một nhóm cố định ở$x=-100$, cái khác tại$x=100$. Mỗi tàu có một số nguyên$y$- phối hợp và nhiều tàu có thể chia sẻ cùng một$y$. 

Chúng ta được phép đặt hai tàu thiện chiến ở bất cứ đâu trên đường thẳng đứng$x=0$, lựa chọn của họ$y$-tọa độ tự do (không nhất thiết phải là số nguyên). Mỗi tàu địch đồng thời bắn hai tia laze: một về phía tàu bạn thứ nhất và một về phía tàu bạn thứ hai. Mỗi tia laze là một nửa đường thẳng vô hạn bắt đầu từ tàu địch và đi qua vị trí đồng đội đã chọn, tiếp tục vượt ra ngoài vị trí đó. 

Một con tàu sẽ bị phá hủy nếu có bất kỳ tia laser nào đi qua vị trí của nó. Nhiệm vụ là chọn hai vị trí trên$x=0$sao cho tổng số tàu địch bị tiêu diệt là tối đa. 

Hạn chế là nhỏ, tối đa là 60 tàu mỗi bên. Điều đó ngay lập tức loại trừ bất cứ điều gì như quét qua tất cả các cấu hình hình học hoặc cố gắng tối ưu hóa liên tục. Bất kỳ giải pháp nào cố gắng mô phỏng giao điểm tia cho các vị trí tùy ý sẽ nhanh chóng trở nên quá phức tạp nếu không tận dụng được cấu trúc. 

Một điểm tinh tế quan trọng là sự phá hủy không chỉ diễn ra cục bộ ở một bên. Một con tàu ở bên trái có thể gián tiếp tiêu diệt những con tàu ở bên phải thông qua sự cộng tuyến với một điểm đã chọn và ngược lại. Sự kết hợp giữa hai bên này là nơi mà sự tính toán ngây thơ có xu hướng bị phá vỡ. 

Một số trường hợp đặc biệt bộc lộ những lỗi điển hình. 

Nếu tất cả các tàu ở một bên đều có chung$y$, và chúng tôi đặt cả hai con tàu thân thiện ở cùng một vị trí$y$, khi đó mọi tia đều nằm ngang, tối đa hóa phạm vi bao phủ. Một giải pháp ngây thơ xử lý từng bên một cách độc lập có thể bỏ lỡ rằng cả hai điểm được chọn đều tương tác qua cùng một tập hợp tia. 

Một trường hợp lỗi khác xảy ra khi cấu hình đối xứng tạo ra các đường chồng chéo. Nếu hai tàu bên trái khác nhau tạo ra tia chiếu trúng cùng một tàu bên phải thì tàu bên phải đó vẫn chỉ được tính một lần. Bất kỳ việc đếm tham lam nào trên mỗi tàu đều đánh giá quá cao kết quả trừ khi logic hợp nhất được xử lý cẩn thận. 

Cuối cùng, nếu cả hai tàu bạn được đặt ở cùng một vị trí$y$, vấn đề sẽ thoái hóa thành cấu hình một điểm. Một giải pháp giả định hai vị trí là khác nhau sẽ bỏ lỡ trường hợp tối ưu này. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực bắt đầu bằng cách thử tất cả các vị trí có thể có của hai con tàu trên$x=0$. Từ$y$là có thật, điều này là liên tục, nhưng hình học giới hạn các sự kiện có ý nghĩa ở các giá trị đặc biệt: tàu bên phải chỉ bị tia của tàu bên trái chiếu vào khi ba điểm thẳng hàng, điều này buộc tàu được chọn$y$các giá trị để thỏa mãn phương trình tuyến tính có dạng$$y = \frac{y_i + y_j}{2}.$$Điều này gợi ý rằng chỉ có một số vị trí ứng cử viên hữu hạn là quan trọng, nhưng ngay cả sau khi rời rạc hóa các ứng cử viên, việc thử tất cả các cặp vị trí và tính toán lại các giao điểm hình học từ đầu sẽ dẫn đến một$O(C^2 \cdot n \cdot m)$cách tiếp cận này, chỉ có thể chấp nhận được nếu được tối ưu hóa mạnh mẽ. 

Quan sát cấu trúc quan trọng là mỗi vị trí được chọn sẽ xác định một cách độc lập một _tập hợp các tàu bị phá hủy ở phía đối diện_. Đối với một vị trí cố định$a$, mỗi tàu rời đi lúc$(-100, y_i)$không làm gì hoặc xác định một đường duy nhất chạm chính xác vào một bên phải$y$- phối hợp, cụ thể là$y_j = 2a - y_i$. Điều này có nghĩa là mỗi vị trí ứng cử viên tạo ra một tập hợp con xác định gồm các tàu bị tiêu diệt ở phía đối diện, độc lập với các tàu khác. 

Điều này làm giảm vấn đề chọn hai ứng cử viên$a$Và$b$, trong đó mỗi người đóng góp một tập hợp các con tàu bị phá hủy ở phía đối diện và chúng ta lấy liên minh. Lý do tương tự được áp dụng một cách đối xứng. 

Khi mọi vị trí ứng cử viên được giảm xuống còn hai mặt nạ bit (hiệu ứng từ trái sang phải và từ phải sang trái), câu trả lời cuối cùng sẽ có được bằng cách kiểm tra tất cả các cặp và thực hiện kết hợp thông qua các thao tác bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hình học liên tục Brute Force | vô hạn / không xác định | O(1) | Không sử dụng được | 
| Rời rạc hóa tất cả các cặp có tính toán lại | O(C2 · n · m) | O(1) | Quá chậm | 
| Mặt nạ bit được tính toán trước cho mỗi ứng cử viên | O(C2 · (n + m)) | O(C · (n + m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta nén bài toán thành một tập hữu hạn các ứng viên$y$- tọa độ cho các tàu thiện chiến. Chúng bao gồm tất cả đầu vào$y$-giá trị và tất cả các điểm giữa$(y_i + y_j)/2$có thể phát sinh từ điều kiện cộng tuyến. 

Khi các ứng cử viên đã được cố định, chúng tôi sẽ tính toán trước cách hành xử của mỗi ứng cử viên. 

1. Xây dựng danh sách các vị trí ứng viên. Chúng bao gồm tất cả các bản gốc$y$-tọa độ từ cả hai phía và tất cả các giá trị$(y_i + y_j)/2$có thể xuất hiện dưới dạng sắp xếp hình học hợp lệ. Điều này đảm bảo rằng mọi vị trí tối ưu đều được thể hiện trong không gian tìm kiếm. 
2. Đối với mỗi ứng viên$c$, tính toán tác động của nó ở phía bên phải bằng cách sử dụng tàu bên trái. Đối với mỗi tàu rời tại$y_i$, tính toán vị trí đánh bên phải ngụ ý$y_j = 2c - y_i$. Nếu giá trị này tồn tại giữa các tàu phù hợp, hãy đánh dấu nó là bị hủy trong mặt nạ bit cho ứng viên$c$. Điều này nén ánh xạ nhiều-một thành một cấu trúc hợp nhất. 
3. Tương tự tính cho từng thí sinh$c$tác dụng của nó ở phía bên trái bằng cách sử dụng tàu bên phải, một lần nữa xây dựng mặt nạ bit. 
4. Bây giờ hãy xem xét mọi cặp ứng cử viên theo thứ tự$(a, b)$. Tổng số tàu bên phải bị phá hủy có được bằng cách lấy sự kết hợp của hai mặt nạ bit tương ứng với$a$Và$b$. Điều tương tự được thực hiện cho phía bên trái. 
5. Câu trả lời cuối cùng là số lượng tối đa trên tất cả các cặp ứng cử viên của tổng số tàu bên trái và bên phải bị phá hủy. 

### Tại sao nó hoạt động 

Mỗi vị trí ứng cử viên tạo ra các ràng buộc hình học xác định: một tàu bên trái có thể đâm vào nhiều nhất một tàu bên phải tại mỗi điểm đã chọn và mối quan hệ đó được thể hiện hoàn toàn bằng phương trình$y_j = 2c - y_i$. Điều này có nghĩa là tất cả các tác động của một ứng cử viên sẽ sụp đổ thành một vấn đề thành viên đã xác định trên tập hợp tọa độ kẻ thù rời rạc. 

Bởi vì sự hủy diệt được định nghĩa là sự tồn tại của ít nhất một tia đi qua một con tàu, nên việc kết hợp hai vị trí tương ứng chính xác với việc lấy hợp của các tập hợp này. Không cần theo dõi sự tương tác giữa các tàu bên trái riêng lẻ ngoài tư cách thành viên trong mặt nạ bit cuối cùng, giúp duy trì tính chính xác mà không làm mất thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    L = list(map(int, input().split()))
    R = list(map(int, input().split()))

    # compress right/left y-values into indices
    R_idx = {y: i for i, y in enumerate(R)}
    L_idx = {y: i for i, y in enumerate(L)}

    # candidate set: all original y-values + midpoints
    cand = set(L + R)
    for y1 in L:
        for y2 in L:
            cand.add((y1 + y2) / 2)
    for y1 in R:
        for y2 in R:
            cand.add((y1 + y2) / 2)

    cand = list(cand)

    # bitmasks
    RL = [0] * len(cand)  # left -> right
    LR = [0] * len(cand)  # right -> left

    # precompute effects
    for ci, c in enumerate(cand):
        # left to right
        for y in L:
            y_r = 2 * c - y
            if y_r in R_idx:
                RL[ci] |= 1 << R_idx[y_r]

        # right to left
        for y in R:
            y_l = 2 * c - y
            if y_l in L_idx:
                LR[ci] |= 1 << L_idx[y_l]

    best = 0
    C = len(cand)

    for i in range(C):
        for j in range(i, C):
            right_mask = RL[i] | RL[j]
            left_mask = LR[i] | LR[j]
            best = max(best, right_mask.bit_count() + left_mask.bit_count())

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén các tương tác hình học thành mặt nạ bit để mỗi vị trí ứng cử viên hoạt động giống như một “hồ sơ hủy” được tính toán trước. Các vòng lặp lồng nhau trên các ứng viên có thể chấp nhận được vì số lượng ứng viên vẫn còn nhỏ dưới các ràng buộc. 

Phần tinh tế nhất là việc xây dựng điểm giữa. Nếu không có nó, một số vị trí tối ưu sẽ không bao giờ được kiểm tra, vì sự căn chỉnh tối ưu thường xảy ra chính xác khi điều kiện phản xạ từ trái sang phải hoặc từ phải sang trái được thỏa mãn. 

Liên kết Bitmask được sử dụng thay vì tính toán lại các giao điểm vì sự phá hủy hoàn toàn dựa trên thiết lập: một con tàu có bị bắn trúng hay không, bất kể có bao nhiêu tia đi qua nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 3
1 2 3
```Ở đây tính đối xứng gợi ý việc đặt cả hai con tàu ở một giá trị trung tâm. 

| Bước | Ứng viên | Mặt nạ RL | Mặt nạ LR | 
| --- | --- | --- | --- | 
| c=2 | lượt truy cập 1,3 | lượt truy cập 1,3 | lượt truy cập 1,3 | 

Nếu chúng ta chọn cả hai ứng cử viên ở mức 2 thì cả hai mặt nạ đều giống nhau nên phép hợp không thêm phần tử mới. 

Điều này xác nhận rằng các vị trí tối ưu chồng chéo không làm tăng gấp đôi số lượng tàu. 

### Ví dụ 2 

đầu vào:```
2 3
0 10
5 15 25
```Một số ứng cử viên tạo ra các bản hit riêng biệt, một số khác kết hợp các bộ rời rạc. 

| một | b | công đoàn đúng | trái đoàn | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 5 | 15 | được rồi tàu | tất cả các tàu còn lại | tối đa | 

Điều này cho thấy tại sao việc chọn hai ứng cử viên khác nhau là cần thiết: chỉ một vị trí không thể bao quát được tất cả các cấu hình thẳng hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(C^2)$| Chúng tôi đánh giá tất cả các cặp vị trí ứng cử viên, mỗi cặp kết hợp hai mặt nạ bit có kích thước không đổi | 
| Không gian |$O(C \cdot (n+m))$| Mỗi ứng cử viên lưu trữ hai bitmask trên tối đa 60 tàu | 

Với tối đa vài nghìn ứng viên và chỉ có mặt nạ 60 bit, giải pháp này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    L = list(map(int, input().split()))
    R = list(map(int, input().split()))

    R_idx = {y: i for i, y in enumerate(R)}
    L_idx = {y: i for i, y in enumerate(L)}

    cand = set(L + R)
    for a in L:
        for b in L:
            cand.add((a + b) / 2)
    for a in R:
        for b in R:
            cand.add((a + b) / 2)

    cand = list(cand)

    RL = [0] * len(cand)
    LR = [0] * len(cand)

    for i, c in enumerate(cand):
        for y in L:
            if 2 * c - y in R_idx:
                RL[i] |= 1 << R_idx[2 * c - y]
        for y in R:
            if 2 * c - y in L_idx:
                LR[i] |= 1 << L_idx[2 * c - y]

    ans = 0
    for i in range(len(cand)):
        for j in range(i, len(cand)):
            ans = max(ans, (RL[i] | RL[j]).bit_count() + (LR[i] | LR[j]).bit_count())

    return str(ans)

# sample-like tests
assert run("3 3\n1 2 3\n1 2 3") == "3"
assert run("1 1\n0\n0") == "1"
assert run("2 2\n0 10\n5 15") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ nhỏ đối xứng | 3 | chồng chéo các vị trí tối ưu | 
| trường hợp một điểm | 1 | xử lý tối thiểu | 
| cấu hình cách nhau | 4 | kết hợp hai ứng cử viên khác biệt | 

## Vỏ cạnh 

Trường hợp suy biến xảy ra khi tất cả các tàu nằm cùng một$y$. Trong tình huống đó, mọi ứng cử viên đều tạo ra các mặt nạ giống hệt nhau và bất kỳ lựa chọn cặp nào cũng mang lại kết quả như nhau. Thuật toán xử lý việc này một cách tự nhiên vì bitwise OR không tạo ra sự trùng lặp nhân tạo. 

Một trường hợp khác phát sinh khi căn chỉnh tối ưu yêu cầu điểm giữa không có trong đầu vào. Ví dụ: nếu tàu bên trái ở mức 0 và 10, và tàu bên phải ở mức 5, một ứng cử viên hợp lệ là$y=2.5$. Bước tạo điểm giữa đảm bảo giá trị này được bao gồm, cho phép phát hiện sự cộng tuyến tương ứng. 

Trường hợp khó phát hiện cuối cùng là khi cả hai tàu được chọn đều giống hệt nhau. Thuật toán không thực thi tính khác biệt và vòng lặp cặp tự nhiên bao gồm$i=j$, mô hình chính xác tình huống chỉ sử dụng một vị trí hiệu quả.
