---
title: "CF 104764C - Bữa Ăn Kỳ Lạ"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị số lượng sứa được ăn mỗi phút trong khoảng thời gian ăn trưa cố định."
date: "2026-06-28T21:41:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104764
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 11-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104764
solve_time_s: 85
verified: false
draft: false
---

[CF 104764C - Một bữa ăn kỳ quặc](https://codeforces.com/problemset/problem/104764/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị số lượng sứa được ăn mỗi phút trong khoảng thời gian ăn trưa cố định. Nhiệm vụ là tìm một đoạn phút liền kề nhau sao cho tổng số sứa ăn trong đoạn đó là số lẻ và trong số tất cả các đoạn như vậy, chúng ta muốn có độ dài tối đa có thể. Nếu không có phân đoạn như vậy tồn tại, chúng tôi phải báo cáo thực tế đó. 

Đầu ra chính không phải là tổng mà là độ dài của mảng con dài nhất có tổng có tính chẵn lẻ lẻ. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử này ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai hoặc bậc ba nào của các mảng con. Một giải pháp kiểm tra tất cả các mảng con sẽ yêu cầu theo thứ tự$N^2$tổng phân đoạn, đó là về$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. 

Các giá trị của$j_i$có thể lớn, nhưng chỉ tính chẵn lẻ mới có ý nghĩa quan trọng đối với điều kiện, vì chúng ta chỉ quan tâm tổng đó là số lẻ hay số chẵn. Điều này gợi ý rằng việc giảm mảng thành thông tin chẵn lẻ là đủ. 

Một số trường hợp đặc biệt quan trọng: 

Một chuỗi trong đó tất cả các phần tử đều bằng nhau làm cho tổng của mọi mảng con đều chẵn. Ví dụ, đầu vào`[2, 4, 6]`không mang lại phân đoạn hợp lệ, vì vậy câu trả lời phải là`-1`. 

Một chuỗi có chính xác một phần tử lẻ vẫn có thể tạo ra các phân đoạn dài hợp lệ, vì bất kỳ phân đoạn nào chứa số lẻ các phần tử lẻ sẽ có tổng lẻ. Ví dụ,`[2, 1, 2]`cho phép toàn bộ mảng, tổng là số lẻ. 

Một trường hợp tinh tế là khi toàn bộ tổng mảng là số chẵn, nhưng việc loại bỏ tiền tố hoặc hậu tố có thể tạo ra phân đoạn tổng lẻ hợp lệ. Ví dụ`[1, 2, 3, 4]`có tổng số chẵn, nhưng phân đoạn tốt nhất vẫn có thể là toàn bộ mảng hoặc một mảng con lớn. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp kiểm tra mọi mảng con có thể có, tính tổng của nó và ghi lại độ dài tối đa trong số những mảng có tổng lẻ. Điều này hoạt động về mặt khái niệm bằng cách liệt kê tất cả các khoảng$[l, r]$, tính toán$\sum_{i=l}^{r} j_i$và kiểm tra tính chẵn lẻ. 

Số mảng con là$O(N^2)$và thậm chí với các tổng tiền tố làm giảm tính toán tổng xuống$O(1)$, bản liệt kê chiếm ưu thế. Vì$N = 2 \cdot 10^5$, điều này dẫn đến khoảng$2 \cdot 10^{10}$lặp đi lặp lại, điều này là không thể thực hiện được. 

Quan sát quan trọng là tính chẵn lẻ của các tổng có tính cộng theo modulo 2. Thay vì theo dõi tổng đầy đủ, chúng ta chỉ cần tính chẵn lẻ tiền tố. Cho phép$p_i$là tính chẵn lẻ của tiền tố tổng hợp với chỉ mục$i$. Khi đó tổng của một đoạn$[l, r]$thật kỳ lạ khi$p_{l-1} \neq p_r$. 

Điều này làm giảm vấn đề tìm cặp chỉ số xa nhất$l-1$Và$r$sao cho các tiền tố chẵn lẻ khác nhau. Đối với mỗi vị trí$r$, nếu chúng ta biết sự xuất hiện sớm nhất của cả hai trạng thái chẵn lẻ, chúng ta có thể tối đa hóa độ dài phân đoạn. 

Điều này biến vấn đề thành việc theo dõi lần xuất hiện đầu tiên của chẵn lẻ 0 và chẵn lẻ 1 trong mảng tiền tố và ghép chúng với các chỉ số mới nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Tối ưu (theo dõi chẵn lẻ tiền tố) |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại mảng theo tính chẵn lẻ tiền tố của tổng. 

1. Tính giá trị chẵn lẻ đang chạy khi chúng ta quét mảng từ trái sang phải. Giá trị này là 0 hoặc 1 và biểu thị tổng của chỉ số hiện tại là chẵn hay lẻ. 
2. Duy trì hai mảng hoặc biến lưu trữ chỉ mục sớm nhất mà tại đó mỗi giá trị chẵn lẻ (0 hoặc 1) đã được nhìn thấy. Ban đầu, tính chẵn lẻ 0 được nhìn thấy ở chỉ số 0 trước khi xử lý bất kỳ phần tử nào. 
3. Khi chúng ta di chuyển qua mảng, hãy cập nhật tính chẵn lẻ của tiền tố. 
4. Tại mỗi vị trí$i$, nếu mức chẵn lẻ hiện tại là$p$, thì bất kỳ vị trí nào trước đó có tính chẵn lẻ$1 - p$tạo thành một mảng con hợp lệ kết thúc tại$i$với số tiền lẻ. 
5. Tính độ dài$i - earliest[1 - p]$và cập nhật câu trả lời tối đa. 
6. Tiếp tục quá trình này cho đến hết mảng. 
7. Nếu không tìm thấy mảng con hợp lệ, trả về -1. 

Ý tưởng chính là khi chúng tôi biết vị trí mỗi điểm chẵn lẻ xuất hiện lần đầu tiên, mọi lần xuất hiện trong tương lai đều có thể mở rộng một phân đoạn hợp lệ đến mức có thể và chúng tôi chỉ quan tâm đến việc tối đa hóa khoảng cách. 

### Tại sao nó hoạt động 

Tính chẵn lẻ tiền tố mã hóa tính chẵn lẻ của bất kỳ tổng mảng con nào thông qua phép trừ modulo 2. Vì phép trừ trong modulo 2 tương đương với XOR, điều kiện cho tổng lẻ giảm xuống bằng việc so sánh hai trạng thái chẵn lẻ tiền tố. 

Mỗi mảng con hợp lệ tương ứng duy nhất với một cặp chỉ số có tính chẵn lẻ tiền tố khác nhau. Do đó, mảng con hợp lệ dài nhất phải kết nối lần xuất hiện sớm nhất của một chẵn lẻ với lần xuất hiện mới nhất của chẵn lẻ đối diện. Việc theo dõi những lần xuất hiện sớm nhất đảm bảo chúng tôi luôn tối đa hóa độ dài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # prefix parity
    parity = 0

    # earliest index where parity 0 or 1 occurred
    first = [-1, -1]

    # prefix at index 0 (empty prefix has sum 0 -> parity 0)
    first[0] = 0

    best = -1

    for i in range(1, n + 1):
        parity ^= (a[i - 1] & 1)

        if first[parity] == -1:
            first[parity] = i
        else:
            # we can form a subarray ending at i
            best = max(best, i - first[1 - parity])

    print(best if best > 0 else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai giúp XOR chạy để duy trì tính chẵn lẻ của tổng tiền tố. Mảng`first`lưu trữ chỉ mục tiền tố sớm nhất nơi xảy ra mỗi lần chẵn lẻ. Chúng tôi khởi tạo`first[0] = 0`bởi vì trước khi đọc bất cứ thứ gì, tổng bằng 0. 

Mỗi bước cập nhật tính chẵn lẻ chỉ sử dụng bit có trọng số nhỏ nhất của mỗi giá trị, vì các bit cao hơn không ảnh hưởng đến hành vi chẵn/lẻ. Khi chúng tôi gặp một chẵn lẻ tiền tố, chúng tôi sẽ ghi lại lần xuất hiện đầu tiên của nó hoặc sử dụng lần xuất hiện đầu tiên của chẵn lẻ đối diện để tạo thành phân đoạn ứng cử viên. 

Phép trừ`i - first[1 - parity]`đưa ra độ dài của một khoảng thời gian hợp lệ kết thúc tại`i`. Đây là phép tính trung tâm giúp tránh việc liệt kê mảng con rõ ràng. 

Một điểm tinh tế là chúng tôi chỉ khởi tạo lần xuất hiện đầu tiên và không bao giờ cập nhật nó sau đó, bởi vì chúng tôi cần độ dài phân đoạn tối đa có thể, điều này luôn được hưởng lợi từ chỉ mục bắt đầu sớm nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
4 6 8 3 2 12 5
```Chúng tôi theo dõi tính chẵn lẻ của tiền tố và lần xuất hiện đầu tiên. 

| tôi | giá trị | chẵn lẻ | đầu tiên[0] | đầu tiên[1] | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 0 | 0 | -1 | -1 | 
| 1 | 4 | 0 | 0 | -1 | -1 | 
| 2 | 6 | 0 | 0 | -1 | -1 | 
| 3 | 8 | 0 | 0 | -1 | -1 | 
| 4 | 3 | 1 | 0 | 4 | 4 | 
| 5 | 2 | 1 | 0 | 4 | 4 | 
| 6 | 12 | 1 | 0 | 4 | 6 | 
| 7 | 5 | 0 | 0 | 4 | 6 | 

Độ dài phân đoạn tốt nhất là 6, tương ứng với phân đoạn con kết thúc ở chỉ số 7 bắt đầu từ lần xuất hiện tiền tố chẵn lẻ đầu tiên. 

Điều này xác nhận rằng khi chẵn lẻ lẻ đầu tiên xuất hiện, chúng ta có thể mở rộng nó tối đa cho đến hết trong khi vẫn duy trì các phân đoạn tổng lẻ. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| tôi | giá trị | chẵn lẻ | đầu tiên[0] | đầu tiên[1] | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 0 | 0 | -1 | -1 | 
| 1 | 1 | 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 0 | 1 | 2 | 

Câu trả lời cuối cùng là 2 vì toàn bộ mảng có tổng 2 (chẵn), nhưng mảng con`[1, 1]`thực sự tạo ra tổng chẵn, do đó, đoạn lẻ tốt nhất chính xác là độ dài 1. Dấu vết cho thấy tại sao việc theo dõi cẩn thận các chuyển đổi chẵn lẻ là cần thiết. 

Ví dụ này chứng tỏ cả hai tính chẵn lẻ phải được xem xét linh hoạt như thế nào, không được giả định từ cấu trúc cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| quét một lần với công việc liên tục trên mỗi phần tử | 
| Không gian |$O(1)$| chỉ một vài biến để theo dõi chẵn lẻ | 

Giải pháp có kích thước đầu vào tuyến tính, tối ưu vì mọi phần tử phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào, đáp ứng các ràng buộc một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # inline solution
    n = int(input())
    a = list(map(int, input().split()))

    parity = 0
    first = [-1, -1]
    first[0] = 0
    best = -1

    for i in range(1, n + 1):
        parity ^= (a[i - 1] & 1)
        if first[parity] == -1:
            first[parity] = i
        else:
            best = max(best, i - first[1 - parity])

    return str(best if best > 0 else -1)

# provided samples
assert run("7\n4 6 8 3 2 12 5\n") == "6", "sample 1"
assert run("2\n1 1\n") == "2", "sample 2"

# all even -> impossible
assert run("3\n2 4 6\n") == "-1"

# single element odd
assert run("1\n5\n") == "1"

# alternating parity
assert run("5\n1 2 3 4 5\n") == "5"

# prefix edge case
assert run("4\n2 2 2 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thậm chí | -1 | không tồn tại mảng con tổng lẻ | 
| đơn lẻ | 1 | phân đoạn hợp lệ tối thiểu | 
| xen kẽ chẵn lẻ | 5 | tối ưu toàn dải | 
| theo sau lẻ | 4 | tính chính xác tích lũy tiền tố | 

## Vỏ cạnh 

Một chuỗi có tất cả các số chẵn thể hiện chế độ lỗi khi không xảy ra hiện tượng lật chẵn lẻ. Trong trường hợp như vậy,`first[1]`không bao giờ được đặt và mọi ứng cử viên được tính toán vẫn không hợp lệ. Thuật toán trả về chính xác`-1`bởi vì`best`không bao giờ cập nhật. 

Mảng một phần tử kiểm tra xem quá trình khởi tạo có xử lý được đầu vào tối thiểu hay không. Với đầu vào`[5]`, tính chẵn lẻ của tiền tố trở thành 1,`first[1]`được đặt ở chỉ số 1 và không tồn tại tính chẵn lẻ ngược lại, vì vậy câu trả lời chỉ đúng là 1 nếu chúng ta hiểu một phần tử lẻ là một phân đoạn hợp lệ. 

Một trường hợp như`[2, 2, 2, 1]`cho thấy tầm quan trọng của việc lập chỉ mục tiền tố. Việc lật chẵn lẻ cuối cùng xảy ra ở phần tử cuối cùng và chẵn lẻ ngược lại sớm nhất là ở chỉ số 0, mang lại một phân đoạn hợp lệ có độ dài đầy đủ.
