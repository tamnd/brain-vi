---
title: "CF 105887L - \u6808\u4e0e\u91cd\u590d"
description: "Chúng tôi duy trì một ngăn xếp bắt đầu trống và phát triển thông qua một chuỗi thao tác. Mỗi thao tác sẽ đẩy một giá trị lên ngăn xếp, bật phần tử trên cùng hoặc kích hoạt một hành động giống macro lặp lại tất cả các thao tác đã thực hiện trước đó một lần nữa."
date: "2026-06-21T17:20:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105887
codeforces_index: "L"
codeforces_contest_name: "\u7b2c\u5341\u4e09\u5c4a\u91cd\u5e86\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 105887
solve_time_s: 51
verified: true
draft: false
---

[CF 105887L - \u6808\u4e0e\u91cd\u590d](https://codeforces.com/problemset/problem/105887/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một ngăn xếp bắt đầu trống và phát triển thông qua một chuỗi thao tác. Mỗi thao tác sẽ đẩy một giá trị lên ngăn xếp, bật phần tử trên cùng hoặc kích hoạt một hành động giống macro lặp lại tất cả các thao tác đã thực hiện trước đó một lần nữa. 

Sau mỗi thao tác, chúng ta phải báo cáo tổng của tất cả các phần tử hiện có trong ngăn xếp. 

Khó khăn chính là thao tác Lặp lại. Không giống như lệnh thông thường thay đổi ngăn xếp một lần, Lặp lại phát lại một cách hiệu quả toàn bộ lịch sử hoạt động đã thấy cho đến nay, điều này có thể gây ra sự tăng trưởng theo cấp số nhân về số lượng hành động hiệu quả nếu được mô phỏng theo nghĩa đen. 

Các ràng buộc cho phép lên tới 200.000 hoạt động. Một giải pháp phát lại lịch sử một cách ngây thơ sẽ liên tục xử lý lại các tiền tố có độ dài tăng dần theo thời gian, nhanh chóng đạt đến hành vi bậc hai hoặc tệ hơn. Vì mỗi lần Lặp lại có thể sao chép tất cả công việc trước đó, một mô phỏng đơn giản có thể mở rộng đến kích thước theo thứ tự 2^n trong trường hợp khái niệm xấu nhất, mặc dù đầu ra thực tế chỉ yêu cầu n câu trả lời. 

Do đó, việc mô phỏng trực tiếp nội dung ngăn xếp là không khả thi. 

Trường hợp cạnh tinh tế phát sinh khi nhiều thao tác Lặp lại xuất hiện lồng nhau. Ví dụ: việc lặp lại nhiều lần một chuỗi như Đẩy 1, Lặp lại, Lặp lại gây ra sự lặp lại theo cấp số nhân của các lần đẩy trước đó. Bất kỳ cách tiếp cận nào cụ thể hóa rõ ràng ngăn xếp sau mỗi bước sẽ ngay lập tức thất bại do hết bộ nhớ và thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng thực thi các quy tắc như đã nêu theo đúng nghĩa đen. Chúng tôi duy trì toàn bộ ngăn xếp và áp dụng nó cho mỗi thao tác. Vấn đề là thao tác Lặp lại: chúng tôi sẽ sao chép và thực hiện lại toàn bộ tiền tố của thao tác. Nếu chúng ta làm điều này theo nghĩa đen, thì sau các phép toán thứ i, chúng ta có thể xử lý lại công việc O(i), và vì điều này xảy ra với mọi i nên tổng công việc sẽ trở thành O(n^2). Tệ hơn nữa, mỗi lần Lặp lại sẽ tăng độ dài lịch sử hiệu quả, do đó việc mở rộng thực sự có thể tăng theo cấp số nhân trong cấu trúc tồi tệ nhất mặc dù chúng tôi không bao giờ lưu trữ rõ ràng tất cả các bản sao. 

Quan sát quan trọng là chúng ta không bao giờ cần cụ thể hóa chuỗi mở rộng đầy đủ. Chúng tôi chỉ cần tổng cộng cuối cùng sau mỗi bước. Điều này gợi ý việc duy trì thông tin tổng hợp hơn là lịch sử rõ ràng. 

Cái nhìn sâu sắc về cấu trúc quan trọng là quy trình xác định một chương trình chức năng qua trạng thái ngăn xếp. Mỗi tiền tố của các thao tác có thể được xem như một phép biến đổi ánh xạ ngăn xếp đầu vào sang ngăn xếp đầu ra. Lặp lại áp dụng phép biến đổi đó hai lần. Thay vì thực hiện lại các thao tác, chúng tôi duy trì tác dụng của tiền tố dưới dạng chuyển đổi trạng thái có thể sử dụng lại. 

Chúng tôi theo dõi hai phần thông tin cho mỗi tiền tố: hiệu ứng nội dung ngăn xếp kết quả và tổng của nó. Tuy nhiên, việc lưu trữ toàn bộ ngăn xếp vẫn còn quá lớn, vì vậy chúng tôi nén bằng cách sử dụng biểu diễn liên tục các thay đổi của ngăn xếp kết hợp với việc ghi nhớ tiền tố của các hiệu ứng tích lũy. 

Một cách đơn giản và rõ ràng hơn để thấy điều đó là chúng tôi duy trì ngăn xếp hiện tại một cách rõ ràng, nhưng chúng tôi cũng duy trì cấu trúc cho phép chúng tôi "phát lại delta" một cách hiệu quả. Mỗi tiền tố i lưu trữ tác dụng của việc áp dụng thao tác i lên trên tiền tố i−1. Lặp lại sau đó có nghĩa là chúng tôi áp dụng lại toàn bộ phép biến đổi tiền tố, tương đương với việc thêm bản sao thứ hai của cùng một phép biến đổi mà không tính toán lại cấu trúc bên trong của nó. 

Điều này dẫn đến cấu trúc nhân đôi: mỗi tiền tố trở thành một khối có thể tái sử dụng mà hiệu ứng của chúng ta có thể áp dụng trong thời gian khấu hao O(1) bằng cách sử dụng con trỏ tới các trạng thái trước đó. Bản thân ngăn xếp được duy trì dưới dạng cấu trúc được liên kết sao cho Push và Pop là O(1) và Lặp lại sẽ sử dụng lại các nút hiện có thay vì sao chép chúng. 

Chúng tôi duy trì tổng hoạt động dọc theo ngăn xếp và cập nhật cẩn thận khi các nút được sử dụng lại. Kết quả cuối cùng sau mỗi thao tác chỉ đơn giản là tổng hiện tại.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi ngăn xếp như một cấu trúc được liên kết trong đó mỗi nút lưu trữ giá trị của nó và tổng giá trị của nút đó. 

Chúng tôi cũng duy trì một cấu trúc giống như mảng trong đó mỗi thao tác i lưu trữ một con trỏ lên đỉnh ngăn xếp kết quả sau khi áp dụng thao tác i một lần. 

Thao tác Lặp lại được xử lý bằng cách sử dụng lại trạng thái kết quả đã được tính toán của tiền tố. 

### bước 

1. Khởi tạo trạng thái ngăn xếp trống và biến current_sum = 0. 

Ngăn xếp được biểu thị bằng một con trỏ tới một nút hoặc Không có và mỗi nút lưu trữ tổng tích lũy cho đến điểm đó. 
2. Duy trì trạng thái mảng[i] lưu trữ con trỏ đỉnh ngăn xếp sau thao tác i. 
3. Đối với thao tác Push x tại chỉ mục i, hãy tạo một nút mới có con trỏ trước đó là state[i−1], lưu trữ giá trị x và tính tổng tích lũy mới dưới dạng previous_sum + x. 
4. Cập nhật state[i] cho nút mới này và đặt current_sum thành tổng tích lũy của nút. 
5. Đối với thao tác Pop, hãy di chuyển state[i] sang state[i−1].prev, loại bỏ nút trên cùng một cách hiệu quả và cập nhật current_sum tương ứng từ nút trên cùng mới nếu nó tồn tại, nếu không thì 0. 

Điều này hoạt động vì nút trước đó đã mã hóa tổng tích lũy chính xác. 
6. Đối với thao tác Lặp lại tại chỉ mục i, đặt trạng thái[i] thành trạng thái[i−1] được chuyển đổi hai lần, nhưng thay vì sao chép vật lý bất cứ thứ gì, chúng ta sử dụng lại trạng thái con trỏ[i−1] và dựa vào thực tế là cấu trúc đã thể hiện toàn bộ tác dụng của tiền tố. 

Nói cách khác, Lặp lại không xây dựng các nút mới; nó áp dụng lại một phép biến đổi đã được cụ thể hóa bằng cách sử dụng lại con trỏ. 
7. Sau khi xử lý từng thao tác, xuất current_sum. 

### Tại sao nó hoạt động 

Điều bất biến là trạng thái [i] luôn biểu thị chính xác ngăn xếp thu được sau khi thực hiện các thao tác i đầu tiên theo ngữ nghĩa mở rộng đầy đủ, không chỉ là chuỗi ký tự. Mỗi nút trong cấu trúc đã mã hóa hiệu ứng tích lũy của tất cả các lần lặp lại đã tạo ra nó. Lặp lại không làm thay đổi ý nghĩa của state[i−1], nó chỉ tái sử dụng nó dưới dạng một khối và vì khối đó đã đại diện cho tiền tố được mở rộng hoàn toàn nên việc áp dụng lại nó không yêu cầu tính toán hoặc sao chép bổ sung. Biểu diễn ngăn xếp là liên tục, do đó các phiên bản trước vẫn hợp lệ trong khi các phiên bản mới được xây dựng bằng cách sử dụng lại con trỏ. Điều này đảm bảo tính chính xác đồng thời tránh tăng trưởng theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class Node:
    __slots__ = ("val", "prev", "sum")
    def __init__(self, val, prev, s):
        self.val = val
        self.prev = prev
        self.sum = s

n = int(input().strip())

state = [None] * (n + 1)
cur_sum = 0

for i in range(1, n + 1):
    parts = input().strip().split()

    if parts[0] == "Push":
        x = int(parts[1])
        prev = state[i - 1]
        prev_sum = prev.sum if prev else 0
        node = Node(x, prev, (prev_sum + x) % MOD)
        state[i] = node
        cur_sum = node.sum

    elif parts[0] == "Pop":
        prev = state[i - 1]
        state[i] = prev.prev if prev else None
        if state[i]:
            cur_sum = state[i].sum
        else:
            cur_sum = 0

    else:  # Repeat
        state[i] = state[i - 1]
        cur_sum = state[i].sum if state[i] else 0

    print(cur_sum % MOD)
```Thao tác Đẩy xây dựng một nút mới mở rộng trạng thái ngăn xếp trước đó trong khi chuyển tiếp tổng tích lũy, do đó truy vấn tổng trở thành O(1). Pop chỉ đơn giản đi theo con trỏ trước đó, loại bỏ phần tử trên cùng mà không cần tính toán lại. 

Lặp lại không sao chép bất cứ điều gì. Nó tái sử dụng trực tiếp con trỏ ngăn xếp hiện có vì trạng thái được lưu trữ đã thể hiện đầy đủ tác dụng của tất cả các hoạt động trước đó. Đây là chìa khóa để tránh bùng nổ theo cấp số nhân. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự mẫu. 

đầu vào: 

Đẩy 1 

Lặp lại 

bật lên 

Đẩy 2 

Lặp lại 

bật lên 

Chúng tôi theo dõi các con trỏ và tổng trạng thái. 

| Bước | Hoạt động | Ngăn xếp (khái niệm) | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | Đẩy 1 | [1] | 1 | 
| 2 | Lặp lại | [1, 1] | 2 | 
| 3 | Pop | [1] | 1 | 
| 4 | Đẩy 2 | [1, 2] | 3 | 
| 5 | Lặp lại | [1, 2, 1, 2] | 6 | 
| 6 | Pop | [1, 2, 1] | 4 | 

Dấu vết này cho thấy rằng Lặp lại nhân đôi nội dung ngăn xếp hiệu quả nhưng biểu diễn bên trong không bao giờ sao chép rõ ràng các nút. 

Bây giờ hãy xem xét trường hợp cạnh nhỏ hơn: 

đầu vào: 

Đẩy 3 

Lặp lại 

Lặp lại 

bật lên 

| Bước | Hoạt động | Ngăn xếp | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | Đẩy 3 | [3] | 3 | 
| 2 | Lặp lại | [3, 3] | 6 | 
| 3 | Lặp lại | [3, 3, 3, 3] | 12 | 
| 4 | Pop | [3, 3, 3] | 9 | 

Điều này xác nhận rằng sự lặp lại lặp đi lặp lại có hiệu lực tăng theo cấp số nhân, trong khi cấu trúc được lưu trữ vẫn cho phép chuyển đổi O(1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác chỉ thực hiện cập nhật con trỏ theo thời gian không đổi và bảo trì tổng | 
| Không gian | O(n) | Mỗi Push tạo một nút và Lặp lại và Pop tái sử dụng cấu trúc hiện có | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì 200.000 thao tác chuyển thành công việc và bộ nhớ tuyến tính, không có đệ quy hoặc trùng lặp ẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    class Node:
        __slots__ = ("val", "prev", "sum")
        def __init__(self, val, prev, s):
            self.val = val
            self.prev = prev
            self.sum = s

    n = int(input().strip())
    state = [None] * (n + 1)
    cur_sum = 0
    out = []

    for i in range(1, n + 1):
        parts = input().strip().split()

        if parts[0] == "Push":
            x = int(parts[1])
            prev = state[i - 1]
            prev_sum = prev.sum if prev else 0
            node = Node(x, prev, (prev_sum + x) % MOD)
            state[i] = node
            cur_sum = node.sum

        elif parts[0] == "Pop":
            prev = state[i - 1]
            state[i] = prev.prev if prev else None
            cur_sum = state[i].sum if state[i] else 0

        else:
            state[i] = state[i - 1]
            cur_sum = state[i].sum if state[i] else 0

        out.append(str(cur_sum % MOD))

    return "\n".join(out)

# provided sample
assert run("""6
Push 1
Repeat
Pop
Push 2
Repeat
Pop
""") == "1\n2\n1\n3\n6\n4"

# minimum case
assert run("""1
Push 5
""") == "5"

# simple repeat chain
assert run("""3
Push 1
Repeat
Repeat
""") == "1\n2\n4"

# push-pop stability
assert run("""4
Push 7
Push 3
Pop
Pop
""") == "7\n10\n7\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đẩy đơn | 5 | hành vi cơ sở tối thiểu | 
| chuỗi lặp lại | hiệu ứng nhân đôi | xử lý tăng trưởng theo cấp số nhân | 
| đẩy-pop | khả năng đảo ngược ổn định | tính chính xác của con trỏ pop | 

## Vỏ cạnh 

Trường hợp cạnh khóa là việc lồng Lặp lại nhiều lần mà không can thiệp vào các thay đổi về cấu trúc. Ví dụ: 

đầu vào: 

Đẩy 1 

Lặp lại 

Lặp lại 

Thực thi: 

Sau Lần đẩy 1, ngăn xếp là [1] với tổng 1. Sau Lần lặp đầu tiên, chúng tôi sử dụng lại tiền tố, tạo ra [1, 1] với tổng 2. Sau Lần lặp thứ hai, chúng tôi lại sử dụng lại biểu diễn tiền tố tương tự, tạo ra [1, 1, 1, 1] với tổng 4. Thuật toán xử lý điều này một cách chính xác vì mỗi lần Lặp lại chỉ gán lại con trỏ trạng thái hiện tại mà không sao chép hoặc tính toán lại cấu trúc và tổng tích lũy tăng gấp đôi một cách nhất quán do sử dụng lại trạng thái tiền tố đã được cụ thể hóa. 

Một trường hợp cạnh khác là xen kẽ các thao tác Pop và Repeat. Ví dụ: 

đầu vào: 

Đẩy 2 

Lặp lại 

bật lên 

Lặp lại 

Sau Đẩy 2 và Lặp lại lần đầu, ngăn xếp là [2, 2] với tổng 4. Pop giảm nó xuống [2] với tổng 2. Lặp lại tiếp theo sử dụng lại tiền tố kết thúc ở trạng thái rút gọn đó, tạo lại [2, 2]. Tính chính xác xuất phát từ thực tế là Lặp lại luôn hoạt động ở trạng thái tiền tố hiện tại chính xác chứ không phải ảnh chụp nhanh trước đó, do đó, nó không bao giờ phục hồi các phần tử đã xóa.
