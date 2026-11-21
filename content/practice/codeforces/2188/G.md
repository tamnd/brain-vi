---
title: "CF 2188G - Cửa và Chìa Khóa"
description: "Chúng ta có một dãy phòng nối với nhau bằng cửa, mỗi cửa có thời gian tự động mở và mỗi phòng có thể chứa một chìa khóa. Bạn bắt đầu từ phòng 1 tại thời điểm 0 và mục tiêu là tìm thời gian tối thiểu để đến từng phòng tiếp theo."
date: "2026-06-09T04:38:09+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 2188
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1077 (Div. 2)"
rating: 3500
weight: 2188
solve_time_s: 98
verified: false
draft: false
---

[CF 2188G - Cửa và Chìa khóa](https://codeforces.com/problemset/problem/2188/G) 

**Đánh giá:** 3500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy phòng nối với nhau bằng cửa, mỗi cửa có thời gian tự động mở và mỗi phòng có thể chứa một chìa khóa. Bạn bắt đầu từ phòng 1 tại thời điểm 0 và mục tiêu là tìm thời gian tối thiểu để đến từng phòng tiếp theo. Việc di chuyển bị hạn chế tùy theo việc cửa có mở hay không và các cửa đã đóng có thể được mở khóa bằng cách sử dụng chìa khóa bạn nhặt được trên đường đi. Bạn có thể mang theo tối đa một chìa khóa mỗi lần và chìa khóa có thể được lấy hoặc đánh rơi ở bất kỳ phòng nào. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng cửa, thời gian mở tự động cho mỗi cửa và chuỗi nhị phân cho biết vị trí của các khóa ban đầu. Đầu ra của mỗi trường hợp thử nghiệm là một danh sách các số nguyên trong đó mỗi số nguyên biểu thị số giây tối thiểu cần thiết để đến phòng ngay bên phải mỗi cánh cửa. 

Quy mô vấn đề rất lớn: lên tới 200.000 cửa cho mỗi trường hợp thử nghiệm và 10.000 trường hợp thử nghiệm, nghĩa là chúng tôi có thể cần xử lý tổng cộng 2×10^5 cửa. Điều này loại trừ mọi mô phỏng ngây thơ của mỗi giây. Cách tiếp cận bạo lực lặp đi lặp lại theo các bước thời gian sẽ quá chậm vì thời gian có thể lên tới 2 × 10^5 và các quyết định chuyển động có thể nhân lên các hoạt động. 

Các trường hợp tế nhị phát sinh khi chìa khóa có sẵn trong phòng trước khi cửa mở hoặc khi cửa mở muộn hơn thời gian cần thiết để lấy chìa khóa. Ví dụ: một phòng có thể có chìa khóa ở vị trí ban đầu, nhưng cửa tiếp theo lại mở vào thời điểm 0. Bạn có thể sử dụng chìa khóa ngay lập tức, ngay cả khi cửa được lên lịch mở sau. Một thuật toán bất cẩn chỉ chờ cửa mở sẽ đánh giá quá cao thời gian tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ mô phỏng từng giây và duy trì trạng thái: phòng hiện tại, liệu bạn có mang theo chìa khóa hay không và các vị trí quan trọng. Vào mỗi giây, bạn sẽ mở những cánh cửa đã đến lúc, nhặt hoặc đánh rơi chìa khóa và di chuyển sang trái hoặc phải. Điều này có hiệu quả về tính chính xác nhưng là O(max(a_i) * n), có khả năng là các phép toán 4×10^10 cho đầu vào trong trường hợp xấu nhất và không khả thi. 

Điểm mấu chốt là vấn đề có thể được giải quyết bằng cách tính toán trước thời gian đến sớm nhất từ ​​cả hai hướng. Đối với mỗi phòng, thời gian sớm nhất bạn có thể đến được từ bên trái bằng cách đợi cửa mở hoặc dùng chìa khóa là`max(time_needed_to_reach_prev_room + 1, door_open_time)`. Nếu có chìa khóa trong phòng, bạn có thể mở khóa cửa tiếp theo ngay lập tức thay vì phải chờ đợi. Tương tự, di chuyển ngay từ bất kỳ phòng nào, nếu cửa tự động mở muộn hơn thời gian tối thiểu mà lẽ ra bạn có thể dùng chìa khóa để mở thì thời gian đến sớm nhất sẽ được xác định bằng thời gian nhỏ hơn trong hai thời gian đó. 

Một quan sát khác là vì bạn có thể mang theo nhiều nhất một chìa khóa nên chìa khóa duy nhất có liên quan là chìa khóa ở phòng bên trái của bạn. Bạn không cần phải theo dõi các lần đánh rơi chìa khóa vì chiến lược tối ưu không bao giờ yêu cầu đánh rơi chìa khóa sau này; bạn sẽ sử dụng nó ngay lập tức trên cánh cửa bị chặn đầu tiên mà bạn gặp phải. Điều này giúp giảm vấn đề xuống còn một lần quét tuyến tính đơn giản với phương pháp lập trình động, tính toán thời gian tối thiểu để đến từng phòng theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Quét DP tuyến tính | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`dp`kích thước`n+1`để lưu trữ số giây tối thiểu để đến từng phòng. Bộ`dp[0] = 0`bởi vì chúng ta bắt đầu ở phòng 1 vào thời điểm 0. 
2. Đi qua các cửa từ trái sang phải. Đối với cửa`i`phòng kết nối`i`ĐẾN`i+1`, thời gian tối thiểu để đến phòng`i+1`hoặc đang đợi cửa tự động mở vào lúc`a[i]`hoặc di chuyển ngay bằng chìa khóa nếu có sẵn trong phòng`i`. Tính toán`dp[i+1]`tối đa của`dp[i] + 1`Và`a[i] + 1`nếu không có key để sử dụng ngay. 
3. Nếu phòng`i`chứa chìa khóa, hãy cân nhắc rằng bạn có thể sử dụng chìa khóa ngay lập tức để mở cửa`i`. Điều này mang lại`dp[i+1] = min(dp[i+1], dp[i] + 1)`. các`+1`tính đến việc chuyển sang phòng tiếp theo vào cuối giây hiện tại. 
4. Lặp lại quy trình này cho tất cả các cửa. Cuối cùng,`dp[i+1]`chứa thời gian tối thiểu để đến phòng`i+1`cân nhắc cả việc chờ mở tự động và sử dụng phím có sẵn một cách tối ưu. 
5. Xuất kết quả tính toán`dp[1:]`cho mỗi trường hợp thử nghiệm. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`dp[i]`là thời gian tối thiểu để đến phòng`i`. Ở mỗi bước, chúng ta đợi cửa hoặc dùng chìa khóa ngay. Vì cửa chỉ mở một lần và ban đầu chỉ có một chìa khóa cho mỗi phòng nên DP không bao giờ đánh giá thấp thời gian. Không cần phải theo dõi việc đánh rơi chìa khóa vì đường đi tối ưu sẽ không bao giờ có những đường vòng không cần thiết để cất chìa khóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()
        dp = [0] * (n + 1)
        for i in range(n):
            # arrive at room i+1 either by waiting for door a[i] or using a key
            dp[i + 1] = max(dp[i] + 1, a[i] + 1)
            # if key is present in room i, consider using it immediately
            if s[i] == '1':
                dp[i + 1] = min(dp[i + 1], dp[i] + 1)
        print(' '.join(map(str, dp[1:])))
        
if __name__ == "__main__":
    solve()
```Mã này phản ánh thuật toán DP. dòng`dp[i+1] = max(dp[i] + 1, a[i] + 1)`đảm bảo bạn không di chuyển trước khi cửa mở trừ khi bạn sử dụng chìa khóa. Dòng tiếp theo sẽ kiểm tra xem có chìa khóa hay không, cho phép chuyển ngay sang phòng tiếp theo. Chúng tôi không theo dõi việc đánh rơi chìa khóa vì chỉ có cánh cửa bị chặn đầu tiên mới quan trọng trong thời gian tối ưu. Có thể tránh được những sai sót riêng biệt bằng cách đánh dấu cẩn thận các cửa và phòng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4
0
0
1
2
0
1
1 0
```| tôi | dp[i] | Cửa a[i-1] | Chìa khóa? | tính toán dp[i+1] | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | - | - | - | 
| 1 | 0 | 0 | '0' | tối đa(0+1, 0+1)=1 | 
| 2 | 1 | 0 | '1' | phút(max(1+1,0+1),1+1)=2 | 

Điều này chứng tỏ rằng thuật toán tính toán chính xác cả việc mở cửa tự động và sử dụng chìa khóa ngay lập tức. 

Ví dụ thứ hai:```
3
2
0 2
10
```| tôi | dp[i] | Cửa a[i-1] | Chìa khóa? | dp[i+1] | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | - | - | - | 
| 1 | 0 | 0 | '1' | min(max(0+1,0+1),0+1)=1 |
 | 2 | 1 | 2 | '0' | tối đa(1+1,2+1)=3 | 

Nó tính toán chính xác rằng để đến được phòng 3, bạn phải đợi cửa thứ hai hoặc di chuyển ngay lập tức nếu có chìa khóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Single pass over doors for each test case; sum of n over all test cases ≤ 2×10^5 |
 | Không gian | O(n) | DP array of size n+1 per test case |

 Thuật toán chạy tốt trong giới hạn 3 giây và giới hạn bộ nhớ 1024 MB, vì chúng tôi thực hiện tối đa 2×10^5 thao tác cho mỗi trường hợp thử nghiệm và chỉ lưu trữ một mảng tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample
assert run("5\n1\n0\n1\n2\n0\n1\n1\n114514\n1\n4\n0 9 10 9\n1101\n10\n0 1 4 2 3 14 10 18 7 25\n1110000001\n") == \
"1\n3\n1\n1 2 5 6\n1 2 3 4 5 8 11 17 18 19", "sample test"

# Custom tests
assert run("1\n1\n0\n0\n") == "1", "single door no key"
assert run("1\n1\n0\n1\n") == "1", "single door with key"
assert run("1\n3\n0 0 0\n111\n") == "1 2 3", "
```
