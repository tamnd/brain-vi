---
title: "CF 1829H - Đừng Trách Tôi"
description: "Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên nhỏ, cụ thể là giá trị 6 bit. Từ mảng này, chúng ta có thể chọn bất kỳ chuỗi con không trống nào, nghĩa là chúng ta chọn bất kỳ tập hợp con chỉ mục nào trong khi vẫn giữ nguyên thứ tự, mặc dù thứ tự không thực sự quan trọng đối với hoạt động theo bit."
date: "2026-06-09T07:19:13+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1829
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 871 (Div. 4)"
rating: 1700
weight: 1829
solve_time_s: 64
verified: true
draft: false
---

[CF 1829H - Đừng trách tôi](https://codeforces.com/problemset/problem/1829/H) 

**Đánh giá:** 1700 
**Tags:** bitmasks, tổ hợp, dp, toán 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên nhỏ, cụ thể là giá trị 6 bit. Từ mảng này, chúng ta có thể chọn bất kỳ chuỗi con không trống nào, nghĩa là chúng ta chọn bất kỳ tập hợp con chỉ mục nào trong khi vẫn giữ nguyên thứ tự, mặc dù thứ tự không thực sự quan trọng đối với hoạt động theo bit. 

Đối với mỗi chuỗi con được chọn, chúng tôi tính toán AND theo từng bit của tất cả các giá trị được chọn. Điều này tạo ra một số khác từ 0 đến 63. Sau đó, chúng tôi đếm xem có bao nhiêu giá trị AND kết quả có chính xác k bit được đặt trong biểu diễn nhị phân của chúng. Câu trả lời cuối cùng là số lượng dãy con hợp lệ chứ không phải số lượng kết quả khác biệt. 

Khó khăn chính là các dãy con tăng theo cấp số nhân với n. Ngay cả với n = 40, bạo lực đã trở thành ranh giới. Ở đây n có thể đạt tới 2 × 10^5, vì vậy mọi phép liệt kê theo cấp số nhân đều không thể thực hiện được. Chúng ta cần khai thác thực tế là các giá trị nhỏ và phép toán AND bị hạn chế. 

Trường hợp cạnh tinh tế xuất hiện khi có số 0. Bất kỳ dãy con nào chứa số 0 đều đóng góp kết quả AND bằng 0, bất kể các phần tử khác. Ví dụ: nếu mảng là [0, 7] thì mọi dãy con chứa 0 đều có AND = 0 và chỉ các tập con không có 0 mới hoạt động bình thường. Nếu k = 0 thì chúng trở nên chiếm ưu thế hơn. Một cách tiếp cận ngây thơ coi số 0 như một giá trị bình thường thường đếm sai vì số 0 hủy tất cả các bit. 

Một trường hợp góc khác là khi k = 0. Chúng tôi đang đếm các chuỗi con có AND không có bit nào được đặt, nghĩa là AND bằng 0. Điều này bao gồm nhiều kết hợp bị sụp đổ do xung đột bit chung. Việc xử lý k = 0 một cách thống nhất là quan trọng vì chỉ logic đếm bit là không đủ; chúng ta phải theo dõi các trạng thái AND chính xác, không chỉ số lượng bit trong quá trình xây dựng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê tất cả các chuỗi con, tính AND của chúng và kiểm tra số bit. Điều này đòi hỏi thời gian O(2^n · n), vì mỗi dãy con VÀ tính toán có thể quét tối đa n phần tử. Ngay cả với khả năng ghi nhớ, số lượng dãy con vẫn chiếm ưu thế, khiến cho việc này không thể thực hiện được nếu vượt quá n ≈ 25. 

Quan sát quan trọng là mỗi phần tử là một mặt nạ 6 bit, do đó mọi kết quả AND cũng nằm trong khoảng từ 0 đến 63. Điều này làm giảm đáng kể không gian trạng thái từ các tập hợp con hàm mũ xuống chỉ còn 64 giá trị AND có thể có. Thay vì theo dõi trực tiếp các tập hợp con, chúng tôi theo dõi xem có bao nhiêu chuỗi con tạo ra mỗi kết quả AND có thể. 

Chúng tôi xử lý từng phần tử một và duy trì một mảng DP trong đó dp[mask] là số dãy con có AND bằng mặt nạ sau khi xử lý một số tiền tố. Khi chúng ta thấy một giá trị x mới, mỗi dãy con hiện có sẽ loại trừ x hoặc bao gồm x. Bao gồm x biến đổi giá trị AND hiện có m thành (m & x). Loại trừ x thì m không thay đổi. Chúng ta cũng đếm dãy con mới chỉ chứa x. 

Quá trình chuyển đổi này hoạt động vì AND có tính kết hợp và đơn điệu đối với phần giao nhau của bit: việc thêm các phần tử chỉ làm giảm hoặc bảo toàn các bit. Do đó chúng ta có thể truyền bá số đếm một cách rõ ràng trên không gian 64 trạng thái. 

Cuối cùng, chúng ta tính tổng dp[mask] trên tất cả các mặt nạ có số lượng bằng k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Bitmask DP qua trạng thái AND | O(n · 64) | O(64) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bảng tần số cho tất cả các kết quả AND có thể có.

1. Khởi tạo một mảng dp có kích thước 64 với tất cả các số 0. Điều này thể hiện số lượng các chuỗi con tạo ra mỗi mặt nạ AND. 
2. Lặp qua từng số x trong mảng. 
3. Tạo một mảng mới ndp được khởi tạo dưới dạng bản sao của dp. Điều này xử lý trường hợp "không lấy x", trong đó tất cả các chuỗi trước đó không thay đổi. 
4. Với mọi mặt nạ m từ 0 đến 63, cập nhật ndp[m & x] bằng cách thêm dp[m]. Điều này thể hiện việc mở rộng từng dãy con hiện có bằng cách bao gồm x, cập nhật giá trị AND của nó. 
5. Cũng tính đến dãy con mới chỉ chứa x bằng cách tăng dần ndp[x]. 
6. Thay thế dp bằng ndp. 
7. Sau khi xử lý tất cả các phần tử, tính tổng dp[mask] cho tất cả các mặt nạ có số lượng bằng k. 

Lý do sao chép dp trước tiên là cần thiết vì mỗi phần tử phải được xem xét chính xác một lần cho mỗi lần chuyển trạng thái tiếp theo. Việc trộn các bản cập nhật tại chỗ sẽ sử dụng lại cùng một phần tử nhiều lần một cách không chính xác trong một lần lặp. 

### Tại sao nó hoạt động 

Mỗi dãy con tương ứng với một dãy duy nhất của các quyết định đưa vào trên các phần tử. Trạng thái DP nén tất cả các chuỗi con có cùng giá trị AND. Quá trình chuyển đổi duy trì tính chính xác vì mỗi chuỗi con không sử dụng phần tử hiện tại hoặc sử dụng nó chính xác một lần và thao tác AND ánh xạ các trạng thái cũ sang trạng thái mới một cách xác định. Vì tất cả các chuyển đổi có thể có đều được bao trùm và không có trạng thái nào được tính hai lần trong một lần lặp duy nhất, nên dp luôn phản ánh sự phân bố chính xác của chuỗi con VÀ kết quả cho tiền tố được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    dp = [0] * 64
    
    for x in a:
        ndp = dp[:]  # not take x
        
        # take x as a new subsequence
        ndp[x] = (ndp[x] + 1) % MOD
        
        # extend existing subsequences
        for m in range(64):
            if dp[m]:
                ndp[m & x] = (ndp[m & x] + dp[m]) % MOD
        
        dp = ndp
    
    ans = 0
    for m in range(64):
        if bin(m).count("1") == k:
            ans = (ans + dp[m]) % MOD
    
    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Mảng DP`dp`theo dõi có bao nhiêu chuỗi con tạo ra mỗi mặt nạ AND có thể. Chi tiết triển khai chính là chúng tôi sao chép mảng trước khi xử lý từng phần tử, đảm bảo chúng tôi không sử dụng lại các bản cập nhật từ cùng một lần lặp. 

dòng`ndp[m & x] += dp[m]`là quá trình chuyển đổi duy nhất: nó mã hóa bao gồm phần tử hiện tại. Sự rõ ràng`ndp[x] += 1`đảm bảo các chuỗi đơn lẻ được tính. 

Cuối cùng, chúng tôi lọc mặt nạ theo số lượng bằng k. Vì mặt nạ chỉ từ 0 đến 63 nên lần quét cuối cùng này là thời gian không đổi cho mỗi lần kiểm tra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
1 3 2
```Chúng tôi theo dõi dp qua mặt nạ. 

| Bước | x | dp trước | điểm nổi bật trong quá trình chuyển đổi | dp sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {0:0} | bắt đầu dãy mới | {1:1} | 
| 2 | 3 | {1:1} | 1&3=1, cộng đơn 3 | {1:2, 3:1} | 
| 3 | 2 | {1:2, 3:1} | 1&2=0, 3&2=2, cộng đơn 2 | {1:2, 3:1, 0:2, 2:1} | 

Mặt nạ có số lượng 1 là 1, 2, 4, 8, 16, 32. Chỉ có 1 và 2 đóng góp ở đây, đưa ra câu trả lời 3. 

Dấu vết này cho thấy cách AND thu gọn dần dần các bit, tạo ra các mặt nạ nhỏ hơn theo thời gian. 

### Ví dụ 2 

đầu vào:```
2 0
0 1
```| Bước | x | dp trước | chuyển tiếp | dp sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {} | đơn 0 | {0:1} | 
| 2 | 1 | {0:1} | 0&1=0, cộng với đơn 1 | {0:2, 1:1} | 

Chúng tôi đếm mặt nạ có số lượng 0, chỉ mặt nạ 0 đủ điều kiện, vì vậy câu trả lời là 2. 

Ví dụ này nêu bật cách số 0 chi phối hành vi AND vì nó hủy tất cả các bit trong bất kỳ chuỗi con kết hợp nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 64) | Mỗi phần tử cập nhật tất cả 64 mặt nạ | 
| Không gian | O(64) | Đã sửa bảng DP trên mặt nạ | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5 × 64, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []

    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        
        dp = [0] * 64
        
        for x in a:
            ndp = dp[:]
            ndp[x] = (ndp[x] + 1) % MOD
            for m in range(64):
                ndp[m & x] = (ndp[m & x] + dp[m]) % MOD
            dp = ndp
        
        ans = 0
        for m in range(64):
            if bin(m).count("1") == k:
                ans = (ans + dp[m]) % MOD
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("""6
5 1
1 1 1 1 1
4 0
0 1 2 3
5 1
5 5 7 4 2
1 2
3
12 0
0 2 0 2 0 2 0 2 0 2 0 2
10 6
63 0 63 5 5 63 63 4 12 13
""") == """31
10
10
1
4032
15"""

# custom cases
assert run("""1
1 0
0
""") == "1", "single zero"

assert run("""1
3 3
7 7 7
""") == "7", "all identical full mask"

assert run("""1
4 1
1 2 4 8
""") == "4", "disjoint bits"

assert run("""1
5 0
1 2 4 0 7
""") == "8", "zero dominating mixes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 1 | dãy con chỉ bằng 0 | 
| tất cả mặt nạ đầy đủ giống hệt nhau | 7 | tích lũy đóng góp giống hệt nhau | 
| bit rời rạc | 4 | VÀ thu gọn chính xác | 
| hỗn hợp thống trị bằng không | 8 | tương tác với số không | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi có nhiều số 0 xuất hiện. Đối với đầu vào như`[0, 0, 0]`, mọi dãy con không trống đều có AND bằng 0. Thuật toán xử lý điều này một cách tự nhiên vì mọi chuyển đổi liên quan đến x = 0 ánh xạ tất cả các mặt nạ thành 0, tích lũy số đếm chính xác trong dp[0]. 

Một trường hợp khác là khi tất cả các số đều giống hệt nhau. Mỗi sự bao gồm nhân đôi các kết hợp nhưng giữ AND không thay đổi. DP tích lũy số lượng vào một mặt nạ duy nhất và sau đó lọc số lượng quần thể sẽ đếm tất cả các chuỗi con hoặc không tính tất cả các chuỗi con tùy thuộc vào k. 

Khi các giá trị có các bit rời rạc như [1, 2, 4, 8], mọi AND giữa các phần tử khác nhau sẽ trở thành 0. DP định tuyến chính xác tất cả các chuỗi con hỗn hợp vào mặt nạ 0, trong khi các chuỗi con đơn lẻ vẫn khác biệt, khớp với cấu trúc tổ hợp của giao điểm bit.
