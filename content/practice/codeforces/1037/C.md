---
title: "CF 1037C - Cân bằng"
description: "Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Hãy coi chúng như hai hàng công tắc, trong đó mỗi vị trí đều bật hoặc tắt."
date: "2026-06-16T18:48:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1037
codeforces_index: "C"
codeforces_contest_name: "Manthan, Codefest 18 (rated, Div. 1 + Div. 2)"
rating: 1300
weight: 1037
solve_time_s: 569
verified: true
draft: false
---

[CF 1037C - Cân bằng](https://codeforces.com/problemset/problem/1037/C) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, dây 
**Thời gian giải:** 9m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau. Hãy coi chúng như hai hàng công tắc, trong đó mỗi vị trí đều bật hoặc tắt. Mục tiêu là chuyển đổi hàng đầu tiên thành hàng thứ hai bằng hai loại thao tác: chúng ta có thể hoán đổi hai vị trí bất kỳ trong chuỗi đầu tiên, trả một chi phí bằng khoảng cách giữa các vị trí đó hoặc chúng ta có thể lật một bit với giá đơn vị. 

Khó khăn chính là việc hoán đổi không miễn phí và phụ thuộc vào khoảng cách, do đó việc di chuyển các bit không khớp xung quanh sẽ rất tốn kém khi thực hiện một cách đơn giản. Flips rẻ nhưng chúng thường xuyên thay đổi một chút, vì vậy việc quyết định giữa việc sửa các điểm không khớp thông qua chuyển động hay sửa trực tiếp là sự cân bằng cốt lõi. 

Ràng buộc lên tới một triệu ký tự loại trừ mọi thứ bậc hai hoặc thậm chí$O(n \log n)$với các hằng số nặng. Bất kỳ giải pháp nào về cơ bản đều phải quét các chuỗi với số lần không đổi và duy trì cấu trúc dữ liệu tuyến tính. 

Một ý tưởng ngây thơ nhưng không chính xác là tham lam sửa những chỗ không khớp từ trái sang phải, luôn lật hoặc hoán đổi cục bộ. Ví dụ: nếu chúng tôi thấy có sự không khớp ở chỉ mục$i$, chúng ta có thể lật nó ngay lập tức. Điều này không thành công vì sự không khớp ở một vị trí có thể được giải quyết rẻ hơn bằng cách ghép nó với một vị trí không khớp sau đó và hoán đổi thay vì trả hai lần lật. 

Một thất bại tinh vi khác đến từ việc coi các giao dịch hoán đổi luôn có lợi khi tồn tại hai vị thế không khớp nhau. Nếu chúng ta hoán đổi mà không xem xét đến khoảng cách, chúng ta có thể cho rằng tồn tại một cặp đôi hoàn hảo, nhưng chi phí để mang các bit không khớp lại với nhau có thể vượt quá hai lần lật, đặc biệt khi các cặp không khớp cách xa nhau. 

## Phương pháp tiếp cận 

Vấn đề chỉ dừng lại ở việc hiểu các vị trí không khớp. Tại bất kỳ chỉ số nào, nếu$a[i] = b[i]$, nó không thành vấn đề. Nếu không, chúng ta có một điểm không khớp cần phải sửa bằng cách lật hoặc ghép nối với một điểm không khớp khác và hoán đổi. 

Chúng ta hãy phân loại sự không phù hợp thành hai loại: vị trí trong đó$a[i] = 0, b[i] = 1$, và các vị trí ở đó$a[i] = 1, b[i] = 0$. Gọi chúng là loại A và loại B tương ứng. Việc hoán đổi giữa một A và một B có thể cố định đồng thời cả hai vị trí. 

Ý tưởng vũ phu sẽ thử mọi cách để ghép những cặp không khớp và quyết định nên hoán đổi hay lật. Điều này nhanh chóng trở thành tổ hợp vì có khả năng$O(n)$không khớp, dẫn đến khả năng khớp theo cấp số nhân hoặc ít nhất là vấn đề khớp chi phí tối thiểu trên một đường có trọng số khoảng cách. 

Quan sát chính là cấu trúc một chiều. Nếu chúng ta liệt kê các chỉ số không khớp theo thứ tự, việc ghép các chỉ số không khớp liền kề thuộc loại đối diện luôn là tối ưu bất cứ khi nào sử dụng phép hoán đổi. Bất kỳ sự ghép đôi không liền kề nào cũng có thể được cải thiện hoặc khớp bằng cách xem xét việc giao cắt: hoán đổi các lực không khớp ở xa truyền qua các chỉ số trung gian, điều này chỉ làm tăng chi phí so với việc giải quyết cục bộ. 

Điều này giúp giảm bớt vấn đề trong việc quét chuỗi, duy trì sự không khớp chưa từng có cuối cùng và quyết định xem nên ghép nó với chuỗi hiện tại hay lần lật trả tiền. 

Chúng tôi duy trì cấu trúc dạng xếp chồng của những điểm không khớp chưa được giải quyết. Bất cứ khi nào chúng ta thấy loại đối diện không khớp với đầu ngăn xếp, chúng ta có thể ghép chúng bằng một phép hoán đổi hoặc để chúng lật. Chi phí hoán đổi chính xác là khoảng cách giữa các chỉ số, trong khi hai lần lật có giá 2. Vì vậy, đối với mỗi cặp, chúng tôi lấy mức tối thiểu của hai tùy chọn đó. 

Vì các chỉ số được xử lý theo thứ tự nên khoảng cách được biết ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu ghép nối không phù hợp | Hàm mũ | O(n) | Quá chậm | 
| Tham lam ghép nối những sự không phù hợp liền kề | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải và duy trì danh sách các chuỗi không khớp chưa được giải quyết. 

1. Quét chỉ mục$i$từ 0 đến$n-1$. Nếu như$a[i] = b[i]$, hãy bỏ qua vì nó không yêu cầu hành động nào. 
2. Nếu không có thông tin không khớp nào đang chờ xử lý, hãy đẩy chỉ mục không khớp hiện tại và loại của nó vào một cấu trúc. Chúng tôi ghi nhớ liệu đó là không khớp 0 đến 1 hay không khớp 1 đến 0 vì việc ghép nối chỉ có ý nghĩa giữa các loại đối diện. 
3. Nếu có một loại đối diện không khớp đang chờ xử lý, chúng tôi sẽ xem xét việc khớp nó với chỉ mục hiện tại. Điều này tạo ra chi phí hoán đổi ứng viên bằng$i - j$, Ở đâu$j$là chỉ số không phù hợp trước đó. 
4. So sánh tùy chọn hoán đổi với việc lật cả hai vị trí một cách độc lập, chi phí là 2. Thêm giá trị tối thiểu của hai vị trí này vào câu trả lời và xóa phần không khớp đang chờ xử lý. 
5. Nếu thông tin không khớp đang chờ xử lý cùng loại với loại hiện tại, chúng tôi không thể ghép nối chúng, vì vậy chúng tôi đẩy thông tin không khớp hiện tại và giữ thông tin không khớp trước đó chờ. 
6. Sau khi xử lý tất cả các chỉ số, mọi điểm không khớp còn lại phải được giải quyết riêng lẻ bằng các lần lật, mỗi lần đóng góp 1 cho câu trả lời. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là sự không khớp tạo thành hai chuỗi có thứ tự trên một dòng. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi sao cho việc hoán đổi chỉ xảy ra giữa các kiểu không khớp liên tiếp theo thứ tự chỉ mục. Bất kỳ cặp lai nào cũng có thể được bỏ lai mà không làm tăng chi phí vì chi phí hoán đổi là tuyến tính theo khoảng cách và việc bỏ lai sẽ làm giảm tổng quãng đường di chuyển. Sau khi bị giới hạn ở các cặp liền kề, mọi quyết định sẽ trở thành cục bộ: hoặc trả chi phí hoán đổi cho cặp đó hoặc trả hai lần lật và không có quyết định nào trong tương lai có thể cải thiện lựa chọn này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = input().strip()
    b = input().strip()

    ans = 0

    prev_idx = -1
    prev_type = 0  # +1 for 0->1, -1 for 1->0

    for i in range(n):
        if a[i] == b[i]:
            continue

        cur_type = 1 if a[i] == '0' else -1

        if prev_idx == -1:
            prev_idx = i
            prev_type = cur_type
        else:
            if prev_type != cur_type:
                cost_swap = i - prev_idx
                ans += min(cost_swap, 2)
                prev_idx = -1
            else:
                ans += 1
                prev_idx = i
                prev_type = cur_type

    if prev_idx != -1:
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã sẽ quét một lần qua các chuỗi và chỉ theo dõi tối đa một lỗi không khớp chưa được giải quyết tại một thời điểm. Biến`prev_idx`lưu trữ vị trí không khớp chưa từng có cuối cùng, trong khi`prev_type`lưu trữ hướng của nó. Khi xuất hiện sự không phù hợp về tính tương thích, chúng tôi sẽ đánh giá chi phí ghép nối so với việc chuyển đổi. 

Một điểm tinh tế là khi hai sự không khớp cùng loại thì chúng ta không thể hình thành một phép hoán đổi, vì vậy sự không khớp trước đó phải được giải quyết bằng cách lật ngay lập tức. Điều này ngăn cản việc đưa các trạng thái không tương thích về phía trước và đảm bảo tính chính xác của việc ghép nối tham lam. 

Cuối cùng, bất kỳ sự không phù hợp nào còn sót lại sẽ được thanh toán dưới dạng lật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = 100
b = 001
```Sự không phù hợp xảy ra ở tất cả các chỉ số. 

| tôi | một [tôi] | b[i] | Loại | trạng thái trước | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | -1 | trống | cửa hàng | 0 | 
| 1 | 0 | 0 | bỏ qua | - | - | 0 | 
| 2 | 0 | 1 | +1 | (-1 lúc 0) | hoán đổi và lật → min(2,2)=2 | 2 | 

Câu trả lời cuối cùng là 2. Điều này cho thấy sự ghép đôi rõ ràng giữa các đầu trong đó khoảng cách hoán đổi bằng hai lần lật. 

### Ví dụ 2 

đầu vào:```
n = 4
a = 0101
b = 0011
```| tôi | một [tôi] | b[i] | Loại | trạng thái trước | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | - | trống | bỏ qua | 0 | 
| 1 | 1 | 0 | -1 | trống | cửa hàng | 0 | 
| 2 | 0 | 1 | +1 | (-1 lúc 1) | phí hoán đổi 1 vs 2 lần lật → 1 | 1 | 
| 3 | 1 | 1 | - | không khớp | - | 1 | 

Điều này chứng tỏ rằng các điểm không khớp đối diện liền kề luôn được ghép nối tốt nhất thông qua hoán đổi khi đóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét tuyến tính đơn trên chuỗi | 
| Không gian | O(1) | chỉ một số lượng biến trạng thái không đổi được lưu trữ | 

Giải pháp thực hiện chính xác một lần chuyển qua các chuỗi có độ dài lên tới$10^6$, dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    def input():
        return sys.stdin.readline()
    
    n = int(sys.stdin.readline())
    a = sys.stdin.readline().strip()
    b = sys.stdin.readline().strip()

    ans = 0
    prev_idx = -1
    prev_type = 0

    for i in range(n):
        if a[i] == b[i]:
            continue
        cur_type = 1 if a[i] == '0' else -1
        if prev_idx == -1:
            prev_idx = i
            prev_type = cur_type
        else:
            if prev_type != cur_type:
                ans += min(i - prev_idx, 2)
                prev_idx = -1
            else:
                ans += 1
                prev_idx = i
                prev_type = cur_type

    if prev_idx != -1:
        ans += 1

    return str(ans)

# provided sample
assert run("3\n100\n001\n") == "2"

# all equal
assert run("5\n00000\n00000\n") == "0"

# single flip needed
assert run("1\n0\n1\n") == "1"

# two mismatches far apart
assert run("4\n1000\n0001\n") == "2"

# alternating mismatches
assert run("6\n101010\n010101\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| không khớp đơn | 1 | ốp lưng lật đế | 
| đầu đối xứng | 2 | quyết định hoán đổi và lật ngược | 
| xen kẽ | 3 | logic ghép nối lặp đi lặp lại | 

## Vỏ cạnh 

Đối với các chuỗi không khớp, thuật toán không bao giờ nhập logic ghép nối và ngay lập tức trả về 0 vì không có trạng thái chờ xử lý nào được tạo. 

Đối với một sự không phù hợp như$a = 0, b = 1$, thuật toán sẽ lưu trữ nó và đi đến điểm cuối, sau đó áp dụng một chi phí lật duy nhất, phù hợp với hành vi tối ưu. 

Đối với nhiều điểm không khớp liên tiếp cùng loại, chẳng hạn như tất cả từ 0 đến 1, thuật toán không bao giờ ghép chúng và giải quyết từng điểm bằng các lần lật, điều này là tối ưu vì các hoán đổi không thể sửa các điểm không khớp cùng hướng.
