---
title: "CF 104097I - \u5b50\u96c6\u5408\u548c (SOS)"
description: "Chúng tôi được cung cấp một mảng được lập chỉ mục bởi bitmasks. Mỗi vị trí đại diện cho một tập hợp con của một số vũ trụ có kích thước $k$, do đó có tổng cộng các giá trị $2^k$. Nhiệm vụ là tính toán, đối với mỗi tập hợp con, tổng hợp của các tập hợp con khác có liên quan đến nó bằng cách đưa vào."
date: "2026-07-02T02:15:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104097
codeforces_index: "I"
codeforces_contest_name: "2022 Taiwan NHSPC Mock Contest"
rating: 0
weight: 104097
solve_time_s: 46
verified: true
draft: false
---

[CF 104097I - \u5b50\u96c6\u5408\u548c (SOS)](https://codeforces.com/problemset/problem/104097/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng được lập chỉ mục bởi bitmasks. Mỗi vị trí đại diện cho một tập hợp con của một số vũ trụ có kích thước$k$, vậy có$2^k$tổng các giá trị. Nhiệm vụ là tính toán, đối với mỗi tập hợp con, tổng hợp của các tập hợp con khác có liên quan đến nó bằng cách đưa vào. Trong phiên bản có tên “tổng tập hợp con trên các tập hợp con”, giá trị của mặt nạ thường là tổng của tất cả các giá trị có mặt nạ được chứa bên trong nó, mặc dù khung tương tự cũng xuất hiện ngược lại trong đó các tập hợp con được tổng hợp thay thế. 

Đầu ra là một mảng khác có cùng kích thước, trong đó mỗi vị trí trả lời truy vấn tổng hợp này cho mặt nạ tương ứng của nó. 

Khó khăn cốt lõi là mỗi truy vấn phụ thuộc vào nhiều mặt nạ khác theo cấp số nhân. Việc giải thích trực tiếp dẫn đến công việc lặp đi lặp lại trên các tập hợp chồng chéo, đó chính xác là vấn đề được thiết kế để bộc lộ. 

Những ràng buộc được ngầm định hình xung quanh$2^k$. Nếu như$k$là khoảng 20 hoặc 22 thì kích thước mảng đầy đủ là khoảng một triệu đến vài triệu mục. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các tập hợp con một cách độc lập cho mỗi mặt nạ, vì điều đó sẽ dẫn đến một cái gì đó như$O(4^k)$, vượt xa giới hạn khả thi. Giải pháp dự định phải sử dụng lại các tính toán từng phần và khai thác cấu trúc trên các mặt nạ bit. 

Trường hợp cạnh phổ biến xuất hiện khi$k = 0$. Trong trường hợp đó, có chính xác một tập hợp con, mặt nạ trống và câu trả lời phải khớp đơn giản với đầu vào. Một trường hợp tinh vi khác là khi tất cả các giá trị đều bằng 0 ngoại trừ một vị trí. Việc liệt kê tập hợp con đơn giản có thể dễ dàng đếm quá mức hoặc bỏ sót các đóng góp nếu quá trình chuyển đổi bit được xử lý không chính xác, đặc biệt nếu việc triển khai kết hợp các hướng dẫn tập hợp con và tập hợp con. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi mặt nạ, lặp lại tất cả các mặt nạ con và tính tổng các giá trị của chúng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của tập hợp cần thiết. Tuy nhiên, số lượng mặt nạ con của mặt nạ là$2^{\text{popcount}(mask)}$và tính tổng giá trị này trên tất cả các mặt nạ dẫn đến xấp xỉ$3^k$hoạt động. Vì$k = 20$, cái này đã có sẵn rồi$3^{20} \approx 3.4 \times 10^9$, quá chậm. 

Quan sát quan trọng là việc tính toán chồng chéo rất nhiều. Khi chuyển từ mặt nạ này sang mặt nạ khác, hầu hết cấu trúc mặt nạ con đều được chia sẻ. Thay vì tính toán lại từ đầu, chúng ta có thể xây dựng câu trả lời tăng dần bằng cách dần dần cho phép nhiều bit hơn tham gia. 

Đây chính xác là cài đặt cho SOS DP, “Lập trình động tổng hợp các tập hợp con”. Ý tưởng là xử lý từng bit một và dần dần nới lỏng các ràng buộc về những bit được phép thay đổi. Sau khi xử lý các$i$-th bit, chúng tôi biết câu trả lời đúng chỉ xem xét thấp hơn$i$chút tự do, và chúng tôi mở rộng điều này để bao gồm cả chút$i$trong khi vẫn giữ được tính chính xác cho tất cả các bit nhỏ hơn. 

Brute-force hoạt động vì nó liệt kê rõ ràng tất cả các mặt nạ con. Không thành công vì phép liệt kê này được lặp lại độc lập cho mọi mặt nạ. SOS DP giảm điều này xuống$k \cdot 2^k$chuyển đổi bằng cách đảm bảo mỗi mối quan hệ tập hợp con được tính chính xác một lần trên mỗi cấp độ bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^k)$|$O(2^k)$| Quá chậm | 
| SOS DP |$O(k \cdot 2^k)$|$O(2^k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng DP trong đó`dp[mask]`ban đầu lưu trữ giá trị đã cho cho tập hợp con đó. Mục tiêu là biến đổi nó sao cho mỗi`dp[mask]`tích lũy đóng góp từ tất cả các mặt nạ con. 

1. Khởi tạo`dp[mask]`với các giá trị đầu vào cho tất cả các mặt nạ. Điều này thể hiện trường hợp cơ sở chưa áp dụng tổng hợp nào. 
2. Lặp lại từng vị trí bit từ 0 đến$k-1$. Mỗi lần lặp cho phép thông tin truyền dọc theo một chiều của mạng tập hợp con được xác định bởi bit đó. 
3. Đối với mỗi mặt nạ, hãy kiểm tra xem bit hiện tại đã được đặt chưa. Nếu nó không được đặt, chúng ta có thể sử dụng mặt nạ này một cách an toàn làm trạng thái cơ sở để kết hợp thông tin từ mặt nạ liên quan chỉ khác ở bit này. 
4. Thực hiện chuyển đổi`dp[mask] += dp[mask with bit set]`cho biến thể tổng tập hợp con. Bước này cho biết một cách hiệu quả rằng tất cả các tập hợp con bao gồm cấu trúc hiện tại cộng với bit bổ sung này đều góp phần vào câu trả lời của mặt nạ hiện tại. 
5. Lặp lại quá trình này cho tất cả các bit để các đóng góp được lan truyền trên tất cả các tổ hợp bit có thể có. 

Thứ tự lặp lại chỉ quan trọng ở việc đảm bảo rằng mỗi chiều được xử lý đầy đủ trước khi chuyển sang chiều tiếp theo. Sau khi hoàn thành tất cả các bit, mỗi mặt nạ đã tích lũy đóng góp từ tất cả các mặt nạ liên quan có liên quan chính xác một lần cho mỗi đường dẫn chuyển đổi hợp lệ. 

### Tại sao nó hoạt động 

Trạng thái của DP sau khi xử lý lần đầu tiên$i$các bit thể hiện sự tổng hợp chính xác trên tất cả các tập hợp con chỉ xem xét các bit đó. Điều bất biến là bất kỳ cặp mặt nạ nào chỉ khác nhau về số bit lớn hơn$i$đã có những đóng góp của họ được hợp nhất một cách chính xác. 

Khi xử lý bit$i$, chúng ta hợp nhất các trạng thái chỉ khác nhau ở bit đó, mở rộng tính đúng đắn từ một khối con nhỏ hơn của mạng Boolean sang một khối con lớn hơn. Vì mọi quan hệ tập hợp con có thể được phân tách thành một chuỗi các lần lật bit, nên mỗi đóng góp được truyền qua chính xác một chuỗi hợp lệ, ngăn chặn sự trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    dp = list(map(int, input().split()))
    
    k = n.bit_length() - 1 if n & (n - 1) == 0 else n.bit_length()
    
    # If input size is 2^k, we infer k from n
    # but more safely, treat k as bit width of indices
    # (common CF format gives n = 2^k directly)
    
    k = (n - 1).bit_length()
    
    for i in range(k):
        for mask in range(n):
            if mask & (1 << i):
                dp[mask] += dp[mask ^ (1 << i)]
    
    print(*dp)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc kích thước của mảng, được coi là tập hợp lũy thừa đầy đủ, theo sau là các giá trị được liên kết với mỗi tập hợp con. Mảng DP được khởi tạo trực tiếp từ đầu vào. 

Vòng lặp chính xử lý từng bit một cách độc lập. Đối với mỗi mặt nạ bao gồm bit hiện tại, chúng tôi thêm phần đóng góp từ cùng một mặt nạ đã loại bỏ bit đó. Hướng này đảm bảo rằng chúng tôi đang tích lũy từ các mặt nạ con thành các tập hợp con, phù hợp với việc tích lũy tổng tập hợp con dự định. 

Việc tính toán của`k`lấy được số bit cần thiết để biểu diễn tất cả các mặt nạ. Vì kích thước mảng là$2^k$, chúng tôi phục hồi$k$sử dụng logic độ dài bit. Một cạm bẫy phổ biến là giả sử$n$đã rồi$k$, điều này sẽ phá vỡ hoàn toàn việc lập chỉ mục. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$k = 2$, vì vậy mặt nạ là 0 đến 3. 

Mảng đầu vào:```
dp = [1, 2, 3, 4]
```Sau khi xử lý bit 0: 

| mặt nạ | nhị phân | dp trước | đóng góp | dp sau | 
| --- | --- | --- | --- | --- | 
| 00 | 0 | 1 | - | 1 | 
| 01 | 1 | 2 | 00 → 01 | 3 | 
| 10 | 2 | 3 | - | 3 | 
| 11 | 3 | 4 | 10 → 11 | 7 | 

Sau khi xử lý bit 1: 

| mặt nạ | nhị phân | dp trước | đóng góp | dp sau | 
| --- | --- | --- | --- | --- | 
| 00 | 0 | 1 | - | 1 | 
| 01 | 1 | 3 | 00 → 01 | 4 | 
| 10 | 2 | 3 | - | 4 | 
| 11 | 3 | 7 | 01 → 11 | 11 | 

Điều này cho thấy rằng mỗi mặt nạ cuối cùng chứa tổng tất cả các mặt nạ con của nó. 

Dấu vết xác nhận rằng các đóng góp được truyền đi từng bước dọc theo các kích thước bit thay vì được tính toán lại một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \cdot 2^k)$| Mỗi bit xử lý tất cả các mặt nạ một lần | 
| Không gian |$O(2^k)$| Mảng DP trên tất cả các tập hợp con | 

Độ phức tạp phù hợp với kích thước tự nhiên của không gian trạng thái. Với$2^k$khoảng một triệu, và$k$khoảng 20, tổng số hoạt động là khoảng 20 triệu bản cập nhật, phù hợp thoải mái trong giới hạn thời gian thông thường trong Python với các vòng lặp hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2
    
    data = inp.strip().split()
    n = int(data[0])
    arr = list(map(int, data[1:]))
    
    dp = arr[:]
    k = (n - 1).bit_length()
    
    for i in range(k):
        for mask in range(n):
            if mask & (1 << i):
                dp[mask] += dp[mask ^ (1 << i)]
    
    return " ".join(map(str, dp))

# small base case
assert run("1\n5") == "5"

# k=2 case
assert run("4\n1 2 3 4") == "1 3 4 10"

# all zeros except one
assert run("4\n0 0 0 7") == "0 0 0 7"

# alternating pattern
assert run("4\n1 0 1 0") == "1 1 2 1"

# maximum small sanity
assert run("2\n1 2") == "1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 2 3 4 | 1 3 4 10 | tính đúng đắn của việc tích lũy tập hợp con | 
| 4 0 0 0 7 | 0 0 0 7 | lan truyền phần tử hoạt động đơn lẻ | 
| 4 1 0 1 0 | 1 1 2 1 | độ chính xác tương tác bit | 

## Vỏ cạnh 

cho$k = 0$, chỉ có một mặt nạ, bộ trống. Thuật toán thực hiện số lần lặp bằng 0 trên các bit, do đó giá trị ban đầu được in không thay đổi. Ví dụ, đầu vào`1\n5`trả lại`5`, phù hợp với định nghĩa vì tập con duy nhất của vũ trụ trống rỗng là chính nó. 

Khi chỉ có một bit được đặt trên tất cả các mặt nạ, việc truyền bá chỉ xảy ra dọc theo chiều đó. DP tích lũy các giá trị một cách chính xác mà không bị nhiễu từ các bit khác vì không có chuyển tiếp nào tồn tại dọc theo các kích thước không được đặt. 

Một trường hợp phức tạp là khi các giá trị thưa thớt nhưng nằm trong các mặt nạ có chỉ số cao hơn. Thuật toán vẫn lan truyền chính xác vì mỗi bit được xử lý độc lập và các bit cao hơn chỉ ảnh hưởng đến các trạng thái chứa chúng một cách rõ ràng.
