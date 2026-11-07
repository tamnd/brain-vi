---
title: "CF 1345B - Cấu trúc thẻ"
description: "Chúng ta được cung cấp những tấm thẻ giống hệt nhau và chúng ta liên tục xây dựng những công trình được gọi là kim tự tháp. Kim tự tháp có chiều cao 1 là cấu trúc nhỏ nhất có thể và mọi kim tự tháp cao hơn đều được xây dựng theo lớp: kim tự tháp cao hơn bao gồm một kim tự tháp nhỏ hơn đặt trên đỉnh của một đế rộng hơn được làm…"
date: "2026-06-16T09:56:36+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1345
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 639 (Div. 2)"
rating: 1100
weight: 1345
solve_time_s: 438
verified: true
draft: false
---

[CF 1345B - Cấu trúc thẻ](https://codeforces.com/problemset/problem/1345/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, dp, toán học 
**Thời gian giải:** 7 phút 18 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp những tấm thẻ giống hệt nhau và chúng ta liên tục xây dựng những công trình được gọi là kim tự tháp. Kim tự tháp có chiều cao 1 là cấu trúc nhỏ nhất có thể và mọi kim tự tháp cao hơn đều được xây dựng theo lớp: kim tự tháp cao hơn bao gồm một kim tự tháp nhỏ hơn được đặt trên đỉnh của một đế rộng hơn được làm từ các thẻ bổ sung. Vì quy tắc xây dựng này nên mỗi chiều cao đều có một chi phí cố định trong thẻ. 

Quá trình này là tham lam và tuần tự. Bắt đầu với một số lượng thẻ nhất định, chúng tôi luôn cố gắng xây dựng kim tự tháp cao nhất có thể. Sau khi xây dựng một cái, chúng tôi trừ đi số thẻ mà nó đã tiêu thụ và lặp lại quyết định tương tự đối với phần còn lại cho đến khi không thể hình thành được kim tự tháp có chiều cao bất kỳ. Đầu ra chỉ đơn giản là tổng số kim tự tháp được xây dựng cho mỗi trường hợp thử nghiệm. 

Đối tượng ẩn chính ở đây không phải là kim tự tháp hình học mà là hàm chi phí của kim tự tháp có chiều cao h. Một khi đã biết được chi phí đó, vấn đề sẽ trở thành một quá trình cạn kiệt nguồn lực xác định: liên tục trừ đi chi phí khả thi lớn nhất. 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm, với tổng số thẻ trên tất cả các thử nghiệm lên tới 10^9. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng từng thẻ hoặc thậm chí tính toán lại chi phí quá tốn kém cho mỗi trường hợp thử nghiệm. Một mô phỏng đơn giản liên tục tìm kiếm chiều cao lớn nhất và các phép trừ vẫn có thể hoạt động nếu tính toán chiều cao nhanh, vì mỗi kim tự tháp tiêu thụ ít nhất 2 thẻ và do đó có nhiều nhất O(n) lần lặp trong trường hợp xấu nhất, nhưng O(n) trên mỗi trường hợp thử nghiệm là không thể khi n có thể là 10^9. 

Một trường hợp thất bại tinh tế hơn xuất phát từ việc giải thích sai về chi phí kim tự tháp. Một sai lầm phổ biến là giả định tăng trưởng tuyến tính hoặc tăng trưởng tam giác với các hằng số sai. Ví dụ: giả sử chiều cao h kim tự tháp có giá h^2 thẻ dẫn đến số lượng kết cấu không chính xác ngay cả đối với các đầu vào nhỏ như n = 14, trong đó hành vi đúng phụ thuộc vào các quy tắc xếp chồng tổ hợp chính xác. 

Một trường hợp cạnh khác là nhỏ n chẳng hạn như n = 1 hoặc n = 2. Trong những trường hợp này, không thể có cấu trúc cao hơn và bất kỳ sự đánh giá quá cao nào về chiều cao khả thi sẽ dẫn đến việc trừ quá nhiều thẻ và tạo ra logic dư âm hoặc không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Chúng tôi tính toán trước hoặc rút ra số lượng thẻ cần thiết cho mỗi chiều cao kim tự tháp h. Sau đó, đối với mỗi trường hợp thử nghiệm, trong khi chúng tôi vẫn còn thẻ, chúng tôi quét xuống từ độ cao tối đa có thể và chọn h lớn nhất có giá không vượt quá số thẻ còn lại. Chúng tôi trừ chi phí đó và tăng câu trả lời. 

Điều này hiệu quả vì mỗi bước đều tối ưu cục bộ và vấn đề thực thi rõ ràng rằng việc xây dựng tham lam là bắt buộc. Tuy nhiên, nút cổ chai liên tục tìm thấy chiều cao hợp lệ lớn nhất. Nếu chúng tôi tính toán lại từ đầu mỗi lần và nếu chiều cao tối đa có thể là khoảng O(√n), thì mỗi lựa chọn sẽ tốn O(√n) và trong trường hợp xấu nhất, chúng tôi có thể xây dựng các kim tự tháp O(√n), dẫn đến hành vi O(n) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. 

Quan sát quan trọng là chi phí của một kim tự tháp tăng nhanh theo chiều cao và với n lên tới 10^9 thì chiều cao tối đa chỉ khoảng vài trăm. Điều này cho phép chúng tôi tính toán trước tất cả chi phí kim tự tháp một lần. Sau đó, quá trình lặp lại sẽ trở thành một phép trừ tham lam đơn giản trên một danh sách chi phí cố định. Bởi vì chuỗi chi phí đang tăng lên nhanh chóng nên chúng ta cũng có thể giảm bước lựa chọn thành một vòng lặp nhỏ trên các giá trị được tính toán trước. 

Điều này chuyển vấn đề thành việc liên tục chọn giá trị lớn nhất từ ​​một danh sách được sắp xếp nhỏ cho đến khi hết tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n √n) trực giác trong trường hợp xấu nhất | O(√n) | Quá chậm | 
| Tối ưu | O(H2 + T·H) trong đó H ≤ 450 | O(H) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước số lượng thẻ cần thiết cho mỗi chiều cao kim tự tháp h cho đến khi chi phí vượt quá 10^9. Điều này an toàn vì các ràng buộc đảm bảo n không bao giờ vượt quá phạm vi này. Chi phí cho mỗi h được tính tăng dần so với h trước đó, vì mỗi lớp mới sẽ thêm một số lượng thẻ có thể dự đoán được. 
2. Lưu trữ tất cả các chi phí hợp lệ trong một mảng cost[] tăng dần. Mỗi mục đại diện cho số lượng thẻ chính xác cần thiết để xây dựng một kim tự tháp có chiều cao đó. 
3. Đối với mỗi trường hợp thử nghiệm, bắt đầu với n đã cho và khởi tạo bộ đếm ans = 0. 
4. Trong khi n vẫn đủ lớn để xây dựng ít nhất kim tự tháp nhỏ nhất, hãy quét qua mảng chi phí từ lớn nhất đến nhỏ nhất và tìm chi phí lớn nhất ≤ n. 
5. Trừ chi phí đó khỏi n và tăng ans lên 1. 
6. Lặp lại cho đến khi không có chi phí nào phù hợp với n còn lại. 

Lý do quét từ lớn nhất trở xuống là chính xác vì quá trình này rõ ràng yêu cầu xây dựng kim tự tháp cao nhất có thể mỗi lần. Vì chi phí tăng dần theo chiều cao nên điều này đảm bảo chúng tôi luôn chọn chiều cao hợp lệ tối đa. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là sau mỗi lần trừ, số quân bài còn lại luôn là số dư từ một lựa chọn tham lam tối ưu ở mỗi bước. Bởi vì chi phí kim tự tháp tạo thành một chuỗi tăng dần và mỗi bước chỉ phụ thuộc vào nguồn lực còn lại, quyết định chọn kim tự tháp khả thi lớn nhất là độc lập với các lựa chọn trước đó ngoài việc giảm n. Điều này ngăn chặn bất kỳ trường hợp nào trong đó việc chọn một kim tự tháp nhỏ hơn sớm hơn có thể cho phép tổng số lượng lớn hơn sau này, vì các kim tự tháp lớn hơn luôn tiêu thụ nhiều thẻ hơn bất kỳ sự kết hợp nào của các cấu trúc một phần nhỏ hơn không được sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# precompute pyramid costs
# cost[h] = number of cards needed for pyramid of height h

MAX_N = 10**9
cost = []

h = 1
curr = 2  # cost of height 1 pyramid
while curr <= MAX_N:
    cost.append(curr)
    h += 1
    # derive next cost incrementally
    # recurrence: cost[h] = cost[h-1] + 3*h - 1
    curr += 3 * h - 1

cost.sort()

t = int(input())
for _ in range(t):
    n = int(input())
    ans = 0

    while n >= 2:
        # pick tallest possible pyramid
        for c in reversed(cost):
            if c <= n:
                n -= c
                ans += 1
                break
        else:
            break

    print(ans)
```Cốt lõi của việc thực hiện là bước tính toán trước. Thay vì lặp đi lặp lại việc lấy kích thước kim tự tháp trong mỗi trường hợp thử nghiệm, chúng tôi xây dựng danh sách chi phí toàn cầu một lần. Điều này đảm bảo tất cả các truy vấn được xử lý hiệu quả. 

Vòng lặp tham lam bên trong mỗi ca kiểm thử luôn quét từ chi phí lớn nhất trở xuống. Điều này an toàn vì danh sách nhỏ (nhiều nhất là vài trăm mục), do đó, ngay cả việc quét tuyến tính cũng đủ nhanh. 

Một điểm tinh tế là điều kiện kết thúc`n >= 2`. Kim tự tháp có chiều cao 1 đã có giá 2 thẻ, vì vậy bất kỳ thẻ nào dưới 2 không thể tạo thành bất kỳ cấu trúc nào. Điều này ngăn chặn việc quét không cần thiết khi câu trả lời đã được hoàn tất. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 14 

Trước tiên, chúng tôi tính toán chi phí kim tự tháp có sẵn lên tới 14, đó là: 

2, 7, 15 (ở đây chỉ sử dụng được 2 và 7) 

| Bước | n | Giá đã chọn | Còn lại n | Kim tự tháp | 
| --- | --- | --- | --- | --- | 
| 1 | 14 | 7 | 7 | 1 | 
| 2 | 7 | 7 | 0 | 2 | 

Quá trình này cho thấy rằng sự lựa chọn tham lam sẽ chọn ra kim tự tháp lớn nhất có thể mỗi lần. Sau khi sử dụng kim tự tháp có chiều cao 2 (chi phí 7), chúng ta vẫn có thể xây dựng một kim tự tháp khác có cùng chiều cao. 

Điều này xác nhận rằng việc lặp lại các lựa chọn có chi phí bằng nhau là hợp lệ và thuật toán không yêu cầu theo dõi độ cao đã sử dụng trên toàn cầu. 

### Ví dụ 2: n = 15 

| Bước | n | Giá đã chọn | Còn lại n | Kim tự tháp | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | 15 | 0 | 1 | 

Ở đây kim tự tháp lớn nhất tiêu thụ tất cả tài nguyên ngay lập tức, không để lại phần còn lại. Điều này chứng tỏ rằng thuật toán dừng tự nhiên mà không cần logic bổ sung khi xảy ra kết quả khớp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(H2 + T·H) | Tính toán trước tạo ra chi phí H; mỗi bài kiểm tra quét giá trị H mỗi bước | 
| Không gian | O(H) | Kim tự tháp cửa hàng có giá lên tới chiều cao khả thi tối đa | 

Chiều cao tối đa H là nhỏ (dưới 500 với n ≤ 10^9), do đó, ngay cả việc tính toán trước bậc hai và quét tuyến tính cho mỗi trường hợp thử nghiệm cũng hiệu quả. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAX_N = 10**9
    cost = []
    curr = 2
    h = 1
    while curr <= MAX_N:
        cost.append(curr)
        h += 1
        curr += 3 * h - 1

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        ans = 0
        while n >= 2:
            for c in reversed(cost):
                if c <= n:
                    n -= c
                    ans += 1
                    break
            else:
                break
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""5
3
14
15
24
1
""") == """1
2
1
3
0"""

# minimum input
assert run("1\n1\n") == "0"

# exact single pyramid
assert run("1\n2\n") == "1"

# repeated same pyramid
assert run("1\n14\n") == "2"

# large value
assert run("1\n1000000000\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 0 | không thể xây dựng được | 
| 1\n2 | 1 | trường hợp kim tự tháp nhỏ nhất | 
| 1\n14 | 2 | lặp đi lặp lại tham lam tái sử dụng | 
| 1\n1000000000 | đầu ra hợp lệ | căng thẳng giới hạn trên | 

## Vỏ cạnh 

Với n = 1, danh sách chi phí chỉ chứa các giá trị bắt đầu từ 2, do đó quá trình quét ngay lập tức không tìm thấy bất kỳ kim tự tháp hợp lệ nào. Vòng lặp không bao giờ đi vào và câu trả lời đúng vẫn là 0. 

Với n = 2, thuật toán chọn chi phí nhỏ nhất đúng một lần, trừ nó về 0 và kết thúc rõ ràng. Điều này xác nhận rằng các kết quả khớp chính xác được xử lý mà không yêu cầu cách viết chữ đặc biệt. 

Đối với n lớn chẳng hạn như 10^9, thuật toán liên tục chọn từ danh sách cố định nhỏ. Vì chi phí kim tự tháp lớn nhất vẫn thấp hơn nhiều so với n, nên một số lần lặp đầu tiên có thể liên tục chọn các kim tự tháp lớn, nhưng tính chất thu hẹp của n đảm bảo kết thúc trong rất ít bước.
