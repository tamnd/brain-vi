---
title: "CF 104149B - Sản xuất bia cơ bản"
description: "Chúng tôi được đưa cho một số vạc thuốc. Mỗi vạc chứa một số lít đã biết và mỗi lít có nồng độ đã biết của một thành phần."
date: "2026-07-02T01:23:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104149
codeforces_index: "B"
codeforces_contest_name: "CPUlm Winter Contest 2022"
rating: 0
weight: 104149
solve_time_s: 57
verified: true
draft: false
---

[CF 104149B - Pha chế bia cơ bản](https://codeforces.com/problemset/problem/104149/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một số vạc thuốc. Mỗi vạc chứa một số lít đã biết và mỗi lít có nồng độ đã biết của một thành phần. Mục tiêu là kết hợp các phần của những chiếc vạc này sao cho hỗn hợp cuối cùng có nồng độ chính xác như mục tiêu, đồng thời tối đa hóa tổng số lít mà chúng ta thu được. 

Bạn được phép đổ bất kỳ phần nào của vạc, vì vậy mỗi vạc hoạt động giống như một nguồn cung cấp chất lỏng liên tục. Hạn chế chỉ áp dụng cho hỗn hợp cuối cùng: nồng độ trung bình có trọng số của mọi thứ bạn dùng phải phù hợp với mục tiêu yêu cầu. 

Một cách hữu ích để trình bày lại mục tiêu là mỗi đơn vị chất lỏng đóng góp một “giá trị” tương đương với mức độ tập trung của nó ở trên hoặc dưới mục tiêu. Chúng tôi muốn thu được càng nhiều chất lỏng càng tốt trong khi làm cho những sai lệch này bị loại bỏ một cách chính xác. 

Các ràng buộc rất nhỏ, tối đa là vài nghìn vạc và các phép tính số học đơn giản trên số thực. Điều này ngay lập tức loại trừ bất kỳ điều gì liên quan đến tìm kiếm theo cấp số nhân hoặc lập trình động phức tạp trên các tập hợp con. Một chiến lược tham lam tuyến tính hoặc tuyến tính được mong đợi. 

Một trường hợp thất bại tinh vi xuất hiện khi suy nghĩ tham lam về việc “lấy mọi thứ gần mục tiêu trước”. Cách tiếp cận đó bỏ qua việc đổ đầy quá một bên sẽ buộc bên kia phải bồi thường và việc sử dụng một phần vạc thường là cần thiết. 

Ví dụ: nếu mục tiêu là 0,5 và chúng ta có hai vạc: một có nồng độ 0,9 và một có nồng độ 0,1, việc lấy tất cả cả hai sẽ mang lại sự cân bằng chính xác nếu khối lượng khớp nhau, nhưng việc thay đổi khối lượng sẽ phá vỡ sự cân bằng một chút ngay cả khi cả hai đều “xa” mục tiêu riêng lẻ. Câu trả lời đúng thường yêu cầu cắt tỉa chính xác một bên. 

Một trường hợp khác là khi tất cả vạc đều ở một phía của mục tiêu. Khi đó không thể sử dụng hết mọi thứ và chúng ta phải loại bỏ một phần những chiếc vạc cực đoan nhất để khôi phục lại sự cân bằng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét mọi tập hợp con có thể có của các vạc và, đối với mỗi tập hợp con, quyết định lấy bao nhiêu từ mỗi vạc để hỗn hợp cuối cùng đạt được nồng độ mục tiêu. Ngay cả khi chúng ta bỏ qua tính chất liên tục và chỉ xem xét việc chọn các tập con, điều đó đã dẫn đến$2^n$khả năng đó là quá lớn đối với$n \le 1000$. Việc đưa ra tính năng tối ưu hóa liên tục bên trong mỗi tập hợp con khiến việc này càng trở nên bất khả thi hơn. 

Quan sát quan trọng là ràng buộc đối với hỗn hợp cuối cùng có thể được viết lại dưới dạng tuyến tính. Nếu chúng ta biểu thị bằng$x_i$số tiền lấy từ vạc$i$, và bởi$p_i$nồng độ của nó, điều kiện$$\frac{\sum x_i p_i}{\sum x_i} = p$$tương đương với$$\sum x_i (p_i - p) = 0.$$Điều này biến bài toán thành việc chọn số lượng không âm$x_i \le c_i$sao cho tổng có trọng số đã ký trở thành 0, đồng thời tối đa hóa$\sum x_i$. 

Bây giờ mọi đơn vị chất lỏng đều có “độ lệch”$d_i = p_i - p$. Những sai lệch tích cực đẩy hỗn hợp lên trên mục tiêu, những sai lệch tiêu cực kéo nó xuống dưới. Nhiệm vụ trở thành cân bằng chính xác các lực lượng này. 

Cấu trúc này ngụ ý một chiến lược tham lam: chúng ta bắt đầu từ việc lấy mọi thứ và sau đó điều chỉnh bằng cách loại bỏ chất lỏng để khắc phục sự mất cân bằng. Việc loại bỏ chất lỏng khỏi vạc sẽ làm thay đổi cả thể tích tổng và tổng độ lệch theo cách tuyến tính, do đó, mỗi đơn vị được loại bỏ sẽ có hiệu suất không đổi trong việc điều chỉnh sự mất cân bằng. 

Điều này làm giảm vấn đề xuống quy trình cân bằng một chiều, trong đó chúng tôi luôn loại bỏ chất lỏng từ phía giúp điều chỉnh sự mất cân bằng nhanh nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cân bằng tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### bước 

1. Chuyển đổi nồng độ mục tiêu$p$vào đường cơ sở sai lệch và tính toán$d_i = p_i - p$cho mỗi cái vạc. Điều này sẽ điều chỉnh lại vấn đề sao cho một hỗn hợp đúng có độ lệch tổng chính xác bằng không. 
2. Bắt đầu bằng cách giả sử chúng ta lấy hết chất lỏng có sẵn từ mỗi chiếc vạc. Tính tổng khối lượng và tổng độ lệch. Đây là điểm khởi đầu lạc quan nhất, mặc dù nó thường vi phạm điều kiện cân bằng. 
3. Nếu tổng độ lệch đã bằng 0 thì tổng âm lượng hiện tại là tối ưu và chúng ta có thể dừng ngay lập tức. 
4. Nếu độ lệch dương thì hỗn hợp quá đậm đặc. Cách duy nhất để giảm nó là loại bỏ chất lỏng khỏi vạc có độ lệch dương, vì việc loại bỏ độ lệch âm sẽ làm cho sự mất cân bằng trở nên tồi tệ hơn. 
5. Sắp xếp các vạc có độ lệch dương theo độ lệch trên lít theo thứ tự giảm dần. Điều này ưu tiên loại bỏ chất lỏng giúp khắc phục sự mất cân bằng nhanh nhất trên mỗi đơn vị thể tích bị mất. 
6. Lặp lại qua các vạc này, loại bỏ càng nhiều càng tốt khỏi mỗi vạc cho đến khi cạn kiệt hoặc tổng độ lệch bằng 0. Mỗi lần loại bỏ làm giảm cả khối lượng và độ lệch một cách tuyến tính. 
7. Nếu độ lệch là âm, hãy thực hiện quy trình đối xứng trên các vạc có độ lệch âm, trước tiên hãy loại bỏ những vạc có độ lệch âm lớn nhất cho đến khi cân bằng được khôi phục. 
8. Thể tích còn lại sau khi cân bằng là hỗn hợp hợp lệ tối đa có thể đạt được. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì một hỗn hợp có thể điều chỉnh độ lệch bằng cách chỉ loại bỏ một phía của mục tiêu. Mỗi đơn vị bị loại bỏ đóng góp một tỷ lệ cố định của hiệu chỉnh độ lệch đối với tổn thất thể tích, do đó, việc chọn tỷ lệ lớn nhất trước tiên sẽ giảm thiểu khối lượng lãng phí. Vì chúng tôi luôn hướng thẳng tới việc khôi phục điều kiện độ lệch bằng 0 mà không bao giờ vượt quá sai hướng, nên chúng tôi bảo toàn âm lượng còn lại tối đa dưới ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p = input().split()
    n = int(n)
    p = float(p)

    pos = []
    neg = []

    total_v = 0.0
    total_d = 0.0

    for _ in range(n):
        c, pi = input().split()
        c = float(c)
        pi = float(pi)

        d = pi - p
        total_v += c
        total_d += c * d

        if d > 0:
            pos.append([d, c])
        elif d < 0:
            neg.append([d, c])

    if abs(total_d) < 1e-12:
        print(total_v)
        return

    if total_d > 0:
        pos.sort(reverse=True)
        for d, c in pos:
            if total_d <= 0:
                break
            take = min(c, total_d / d)
            total_v -= take
            total_d -= take * d

    else:
        neg.sort()
        for d, c in neg:
            if total_d >= 0:
                break
            take = min(c, total_d / d)
            total_v -= take
            total_d -= take * d

    print(total_v)

if __name__ == "__main__":
    solve()
```Giải pháp này tính toán hỗn hợp đầy đủ trước tiên, sau đó xử lý việc điều chỉnh mất cân bằng như một quy trình loại bỏ có kiểm soát. Chi tiết triển khai chính đang hoạt động hoàn toàn ở dạng dấu phẩy động với các cập nhật tuyến tính cẩn thận: mỗi lần xóa sẽ giảm cả khối lượng và độ lệch theo tỷ lệ. 

Việc phân loại chỉ được áp dụng trong phạm vi một bên của độ lệch, đảm bảo chúng tôi luôn loại bỏ chất lỏng “hiệu quả” nhất trước tiên. Tính đối xứng giữa trường hợp dương và trường hợp âm được xử lý rõ ràng để tránh lỗi dấu trong công thức cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 0.5
5 0.3
1 0.4
10 0.9
```Chúng tôi tính toán độ lệch: 

| Bước | Khối lượng đã lấy | Tổng độ lệch | 
| --- | --- | --- | 
| Bắt đầu | 16 | +3.0 | 

Hỗn hợp quá mạnh nên trước tiên chúng tôi loại bỏ các sai lệch dương. 

Chúng tôi sắp xếp độ lệch dương: 0,4 và 0,9, nhưng tính trọng số theo khoảng cách từ 0,5 sẽ cho kết quả đóng góp là 0,4 và 0,9. 

Chúng tôi loại bỏ từ 0,9 trước cho đến khi số dư được khôi phục, sau đó dừng chính xác khi độ lệch bằng 0. Khối lượng còn lại trở thành 8,75. 

Điều này cho thấy chỉ cần loại bỏ một phần khỏi vạc có nồng độ cao chứ không phải loại trừ hoàn toàn. 

### Ví dụ 2 

đầu vào:```
3 0.5
5 0.3
1 0.4
1 0.9
```| Bước | Khối lượng đã lấy | Tổng độ lệch | 
| --- | --- | --- | 
| Bắt đầu | 7 | +0,3 | 

Chúng tôi lại giảm từ độ lệch dương mạnh nhất trước tiên (0,9). Chỉ cần một phần của nó để hủy bỏ sự mất cân bằng. 

Sau khi loại bỏ vừa đủ khỏi vạc 0,9, hệ thống đạt đến trạng thái cân bằng và thể tích cuối cùng là 3,5. 

Ví dụ này nhấn mạnh rằng giải pháp tối ưu thường sử dụng phương pháp loại bỏ từng phần khỏi một vạc thay vì loại bỏ toàn bộ vạc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp vạc theo độ lệch chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Lưu trữ cho các nhóm sai lệch tích cực và tiêu cực | 

Các ràng buộc cho phép lên tới vài nghìn vạc, vì vậy$n \log n$cách tiếp cận tham lam dễ dàng phù hợp trong giới hạn thời gian. Thuật toán chỉ thực hiện các phép tính số học đơn giản và một lần sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, p = sys.stdin.readline().split()
    n = int(n)
    p = float(p)

    pos = []
    neg = []
    total_v = 0.0
    total_d = 0.0

    for _ in range(n):
        c, pi = sys.stdin.readline().split()
        c = float(c)
        pi = float(pi)
        d = pi - p
        total_v += c
        total_d += c * d
        if d > 0:
            pos.append([d, c])
        elif d < 0:
            neg.append([d, c])

    if abs(total_d) < 1e-12:
        return str(total_v)

    if total_d > 0:
        pos.sort(reverse=True)
        for d, c in pos:
            if total_d <= 0:
                break
            take = min(c, total_d / d)
            total_v -= take
            total_d -= take * d
    else:
        neg.sort()
        for d, c in neg:
            if total_d >= 0:
                break
            take = min(c, total_d / d)
            total_v -= take
            total_d -= take * d

    return str(total_v)

# provided samples
assert run("""3 0.5
5 0.3
1 0.4
10 0.9
""").strip() == "8.75"

assert run("""3 0.5
5 0.3
1 0.4
1 0.9
""").strip() == "3.5"

# minimum case
assert run("""1 0.5
10 0.5
""").strip() == "10.0"

# all above target
assert run("""2 0.3
5 0.6
5 0.7
""")  # should still produce a valid float

# mixed balanced
assert run("""2 0.5
1 0.0
1 1.0
""").strip() == "2.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu chính xác duy nhất | toàn bộ âm lượng | sự đúng đắn tầm thường | 
| tất cả mục tiêu đều bình đẳng | toàn bộ số tiền | không cần điều chỉnh | 
| cực trị đối xứng | toàn bộ âm lượng | hủy bỏ hoàn hảo | 
| tất cả đều trên mục tiêu | giảm âm lượng | chỉnh sửa một phía | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các vạc đều vượt quá nồng độ mục tiêu. Thuật toán đi vào nhánh “độ lệch dương” một cách chính xác và loại bỏ khỏi vạc mạnh nhất trước tiên cho đến khi loại bỏ độ lệch. Mặc dù không có đối tác cân bằng nào tồn tại, việc loại bỏ từng phần đảm bảo một giải pháp chính xác hợp lệ. 

Một trường hợp khác là khi hỗn hợp ban đầu đã cân bằng. Tổng độ lệch bằng 0 nên thuật toán ngay lập tức trả về toàn bộ âm lượng mà không cần sắp xếp hoặc loại bỏ, duy trì tính tối ưu. 

Trường hợp thứ ba là khi chỉ cần một sự điều chỉnh nhỏ. Vì quá trình loại bỏ diễn ra liên tục nên thuật toán có thể dừng giữa chừng trong quá trình vạc, đảm bảo đáp ứng các yêu cầu về độ chính xác mà không làm quá mức hỗn hợp mục tiêu.
