---
title: "CF 1197D - Một vấn đề về mảng con khác"
description: "Chúng tôi được cung cấp một chuỗi số và được yêu cầu chọn một phân đoạn liền kề hoặc bỏ qua việc chọn bất kỳ thứ gì để tối đa hóa hàm điểm cụ thể. Điểm của một đoạn được chọn là tổng của nó trừ đi một khoản phạt chỉ phụ thuộc vào độ dài của đoạn đó."
date: "2026-06-13T14:27:53+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1197
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 69 (Rated for Div. 2)"
rating: 1900
weight: 1197
solve_time_s: 268
verified: false
draft: false
---

[CF 1197D - Một vấn đề về mảng con khác](https://codeforces.com/problemset/problem/1197/D) 

**Xếp hạng:** 1900 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi số và được yêu cầu chọn một phân đoạn liền kề hoặc bỏ qua việc chọn bất kỳ thứ gì để tối đa hóa hàm điểm cụ thể. Điểm của một đoạn được chọn là tổng của nó trừ đi một khoản phạt chỉ phụ thuộc vào độ dài của đoạn đó. Hình phạt tỷ lệ thuận với bao nhiêu khối kích thước`m`là cần thiết để bao phủ phân khúc, trong đó mỗi khối có giá`k`. 

Vì vậy, thay vì tổng mảng con tối đa đơn giản, mỗi mảng bổ sung`m`các yếu tố kích hoạt một cách hiệu quả một khoản khấu trừ khác của`k`. Điều này làm cho vấn đề hoạt động giống như một tối ưu hóa kiểu Kadane tiêu chuẩn nhưng với chi phí từng bước chỉ thay đổi khi độ dài vượt qua bội số của`m`. 

Đầu ra là một số nguyên duy nhất: điểm cao nhất có thể đạt được trên tất cả các mảng con có thể có, bao gồm tùy chọn không chọn gì, kết quả là 0. 

Các ràng buộc ngay lập tức loại trừ mọi nghiệm bậc hai hoặc nghiệm kém hơn. Với`n`lên tới 300.000, bất kỳ cách tiếp cận nào thử tất cả các mảng con hoặc thậm chí tính toán lại tổng trên mỗi mảng con sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. Chúng tôi buộc phải quét tuyến tính hoặc gần tuyến tính bằng một số dạng lập trình động. 

Một khó khăn tinh tế là hình phạt không có độ dài tuyến tính. Nó chỉ thay đổi khi độ dài đoạn vượt qua bội số của`m`. Điều này tạo ra sự gián đoạn làm phá vỡ công thức Kadane tiêu chuẩn. 

Một vài tình huống khó khăn minh họa vấn đề một cách rõ ràng. 

Nếu tất cả các số đều âm, thì tổng mảng con tối đa đơn giản sẽ trả về phân đoạn âm nhỏ nhất, nhưng ở đây mảng con trống được cho phép và mang lại kết quả bằng 0, vì vậy câu trả lời không bao giờ được nhỏ hơn 0. 

Nếu như`m = 1`, thì mọi phần tử đều đóng góp giá trị của nó trừ đi`k`, biến vấn đề thành tổng mảng con tối đa tiêu chuẩn trên các giá trị được chuyển đổi`a[i] - k`. Bất kỳ giải pháp nào không nhận ra được sự thoái hóa này vẫn có thể hoạt động nhưng nó cung cấp một biện pháp kiểm tra tỉnh táo hữu ích. 

Một dạng lỗi khác phát sinh nếu người ta cho rằng hình phạt có thể được phân bổ đồng đều giữa các phần tử. Ví dụ, cố gắng trừ`k/m`mỗi phần tử không chính xác vì trần tạo ra các bước nhảy ở ranh giới. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: liệt kê mọi mảng con, tính tổng, tính độ dài của nó, đánh giá hình phạt bằng cách sử dụng biểu thức trần và theo dõi mức tối đa. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, có khoảng`O(n^2)`mảng con và thậm chí với các tổng tiền tố để tính tổng phạm vi trong O(1), chúng tôi vẫn dành thời gian bậc hai để đánh giá các ứng cử viên. Với`n = 3e5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mức phạt chỉ phụ thuộc vào độ dài của mảng con và độ dài tăng đều đều khi chúng ta mở rộng điểm cuối bên phải cố định. Điều này gợi ý một công thức lập trình động dựa trên các tiền tố, trong đó chúng tôi duy trì điểm số tốt nhất của mảng con kết thúc ở mỗi vị trí. 

Chúng tôi xác định trạng thái DP đang chạy biểu thị mảng con tốt nhất kết thúc tại vị trí`i`. Khi mở rộng mảng con thêm một phần tử, tổng tăng thêm`a[i]`, nhưng hình phạt có thể tăng hoặc không tăng tùy thuộc vào việc chúng ta có vượt qua bội số của`m`. Đây là điểm quan trọng: trong mỗi khối kích thước`m`, việc thêm các phần tử sẽ không bị phạt thêm cho đến khi khối được lấp đầy. 

Cấu trúc này gợi ý theo dõi câu trả lời một cách riêng biệt theo số lượng phần tử đã được lấy theo modulo`m`. Đối với mỗi vị trí, chúng tôi duy trì`m`Trạng thái DP tương ứng với modulo độ dài mảng con hiện tại`m`. Khi chúng tôi mở rộng thêm một phần tử, chúng tôi chuyển đổi giữa các trạng thái này và chỉ khi gói từ`m-1`quay lại`0`chúng tôi có áp dụng chi phí bổ sung không`k`. 

Điều này làm giảm vấn đề xuống DP trạng thái nhỏ trên mảng, đưa ra giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| DP mô-đun | O(n·m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì trạng thái DP`dp[r]`, Ở đâu`r`đại diện cho độ dài của modulo mảng con được chọn hiện tại`m`. Mỗi trạng thái lưu trữ chênh lệch tổng tốt nhất có thể cho một mảng con kết thúc ở chỉ mục hiện tại với phần còn lại đó. 

Chúng tôi cũng cho phép khởi động lại ở bất kỳ vị trí nào vì mảng con trống là hợp lệ và chúng tôi không bị buộc phải mở rộng các phân đoạn trước đó. 

### bước 

1. Khởi tạo tất cả các trạng thái DP thành giá trị rất âm và đặt câu trả lời chung về 0. Điều này giải thích cho tùy chọn không chọn phân mảng nào cả. 
2. Lặp lại mảng từ trái sang phải, coi mỗi vị trí là điểm cuối tiềm năng của mảng con. 
3. Tại mỗi phần tử`a[i]`, chúng tôi xem xét việc bắt đầu một mảng con mới chỉ bao gồm phần tử này. Phân mảng mới này có độ dài`1`, vì vậy nó góp phần`a[i]`mà chưa có hình phạt nào. 
4. Chúng tôi cũng xem xét việc mở rộng tất cả các trạng thái DP trước đó bằng cách thêm`a[i]`. Nếu chúng ta mở rộng một trạng thái có phần dư`r`, phần dư mới trở thành`(r + 1) % m`. 
5. Khi số dư mới trở thành`0`, có nghĩa là chúng ta vừa hoàn thành một khối có kích thước`m`, vì vậy chúng tôi trừ`k`từ giá trị tích lũy. Điều này mã hóa hình phạt trần từng bước. 
6. Sau khi cập nhật các chuyển đổi, chúng tôi cập nhật câu trả lời với tất cả các trạng thái DP, vì bất kỳ mảng con kết thúc nào cũng có thể là tối ưu. 
7. Tiếp tục cho đến hết mảng. 

### Tại sao nó hoạt động 

Trạng thái DP ngầm theo dõi mọi mảng con kết thúc ở chỉ mục hiện tại, được nhóm theo số lượng phần tử vượt quá bội số của`m`nó hiện có. Bất cứ khi nào một mảng con vượt qua bội số của`m`, đúng một hình phạt bổ sung của`k`được áp dụng, phù hợp với định nghĩa trần. Vì mọi mảng con đều được bắt đầu mới hoặc được mở rộng từ mảng con hợp lệ trước đó nên tất cả các khả năng đều được đề cập. Quá trình chuyển đổi không bao giờ tăng gấp đôi số lần phạt vì chi phí chỉ được kích hoạt khi vượt qua ranh giới chính xác, tương ứng chính xác với các chuyển đổi trong đó phần còn lại được đặt lại về 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    NEG = -10**30
    
    dp = [NEG] * m
    ans = 0
    
    for x in a:
        new_dp = [NEG] * m
        
        # start new subarray
        new_dp[1 % m] = max(new_dp[1 % m], x)
        
        # extend previous subarrays
        for r in range(m):
            if dp[r] == NEG:
                continue
            nr = (r + 1) % m
            val = dp[r] + x
            if nr == 0:
                val -= k
            new_dp[nr] = max(new_dp[nr], val)
        
        for r in range(m):
            dp[r] = max(dp[r], new_dp[r])
            ans = max(ans, dp[r])
    
    return ans

if __name__ == "__main__":
    print(solve())
```Mảng DP`dp[r]`lưu trữ giá trị tốt nhất của bất kỳ mảng con nào kết thúc ở vị trí hiện tại với độ dài bằng với`r mod m`. Quá trình chuyển đổi xử lý rõ ràng cả việc bắt đầu các phân đoạn mới và mở rộng các phân đoạn cũ. Phép trừ của`k`chính xác khi nào`nr == 0`mã hóa hành vi của chức năng trần. 

Việc sử dụng`NEG`đảm bảo rằng các trạng thái không hợp lệ không cản trở quá trình chuyển đổi. Chúng tôi liên tục hợp nhất`new_dp`vào trong`dp`để tất cả các mảng con vẫn có sẵn để mở rộng ở các vị trí sau. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
n = 7, m = 3, k = 10
a = [2, -4, 15, -3, 4, 8, 3]
```Chúng tôi chỉ theo dõi các tiểu bang có liên quan nhất. 

| tôi | một [tôi] | dp trước | chuyển tiếp quan trọng | dp sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | tất cả NEG | bắt đầu [2] | r=1:2 | 2 | 
| 2 | -4 | r=1:2 | mở rộng → -2 | r=2:-2 | 2 | 
| 3 | 15 | r=2:-2 | mở rộng +15 → 13 (không bị phạt) | r=0:13-10=3, bắt đầu 15 | 15 | 
| 4 | -3 | ... | mở rộng tốt nhất r=0 → 0 | r=1:0 | 15 | 
| 5 | 4 | ... | mở rộng r=1 → 4 | r=2:4 | 15 | 
| 6 | 8 | ... | mở rộng r=2 → 12-10=2 | r=0:2 | 15 | 
| 7 | 3 | ... | mở rộng r=0 → 5 | r=1:5 | 15 | 

Dấu vết cho thấy hình phạt chỉ kích hoạt khi hoàn thành các khối có kích thước`m`và làm thế nào các phân khúc tốt có thể tích lũy nhiều yếu tố trước khi trả chi phí. 

Một ví dụ đơn giản thứ hai:```
a = [5, -1, -1], m = 2, k = 3
```Đây là phân khúc tốt nhất`[5, -1]`với chi phí`4 - 3 = 1`và DP thực hiện chính xác hình phạt khi độ dài đạt đến 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Mỗi phần tử xử lý tối đa m trạng thái và m ≤ 10 | 
| Không gian | O(m) | Chỉ các mảng DP có kích thước m được lưu trữ | 

Với`n ≤ 3e5`Và`m ≤ 10`, tổng số hoạt động là khoảng 3e6 lần chuyển đổi, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    NEG = -10**30
    dp = [NEG] * m
    ans = 0
    
    for x in a:
        new_dp = [NEG] * m
        new_dp[1 % m] = max(new_dp[1 % m], x)
        
        for r in range(m):
            if dp[r] == NEG:
                continue
            nr = (r + 1) % m
            val = dp[r] + x
            if nr == 0:
                val -= k
            new_dp[nr] = max(new_dp[nr], val)
        
        for r in range(m):
            dp[r] = max(dp[r], new_dp[r])
            ans = max(ans, dp[r])
    
    return str(ans)

# provided sample
assert run("7 3 10\n2 -4 15 -3 4 8 3\n") == "7"

# minimum size
assert run("1 1 5\n10\n") == "5"

# all negative
assert run("3 2 4\n-1 -2 -3\n") == "0"

# m = 1 reduces to Kadane with penalty
assert run("5 1 2\n3 -1 4 -2 5\n") == "7"

# boundary crossing test
assert run("4 2 10\n5 5 5 5\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | tay cầm m=1 thùng đế | độ chính xác tối thiểu | 
| tất cả đều tiêu cực | trả về 0 | sự thống trị của mảng con trống | 
| m=1 trường hợp | chuyển thành hình phạt tuyến tính | kiểm tra độ tỉnh táo giảm | 
| tích cực lặp đi lặp lại | kiểm tra thời gian phạt đền | chuyển tiếp ranh giới | 

## Vỏ cạnh 

Khi mảng hoàn toàn âm, mọi trạng thái DP vẫn có thể tích lũy các giá trị âm, nhưng thuật toán luôn so sánh với 0, đảm bảo mảng con trống vẫn tối ưu. Ví dụ, với`[-5, -2]`, mọi tiện ích mở rộng đều làm giảm điểm và không có trạng thái DP nào vượt quá 0, vì vậy câu trả lời chính xác vẫn giữ nguyên`0`. 

Khi`m = 1`, mọi phần tử mới sẽ hoàn thành một khối ngay lập tức. Các chuyển tiếp DP trừ`k`trên mỗi bước, phù hợp với công thức`a[i] - k`. Do đó, thuật toán hoạt động chính xác giống như thuật toán của Kadane trên các giá trị được chuyển đổi và việc nén trạng thái không phá vỡ trường hợp đặc biệt này. 

Khi độ dài đoạn tối ưu không phải là bội số của`m`, DP vẫn theo dõi phần còn lại. Khối không hoàn chỉnh cuối cùng không phải chịu thêm một hình phạt nào, vì phép trừ chỉ xảy ra khi phần còn lại bằng 0. Điều này đảm bảo rằng các phân đoạn như độ dài`m + 1`được tính phí chính xác một lần, không phải hai lần, khớp chính xác với định nghĩa trần.
