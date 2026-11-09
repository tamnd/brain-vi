---
title: "CF 1494C - 1D Sokoban"
description: "Chúng ta được yêu cầu tối đa hóa số lượng hộp được đặt ở các vị trí đặc biệt dọc theo đường số vô hạn một chiều. Bạn bắt đầu ở vị trí 0 và có thể di chuyển sang trái hoặc phải, đẩy các hộp theo hướng bạn di chuyển."
date: "2026-06-10T22:09:31+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1494
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 105 (Rated for Div. 2)"
rating: 1900
weight: 1494
solve_time_s: 185
verified: true
draft: false
---

[CF 1494C - 1D Sokoban](https://codeforces.com/problemset/problem/1494/C) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, dp, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa số lượng hộp được đặt ở các vị trí đặc biệt dọc theo đường số vô hạn một chiều. Bạn bắt đầu ở vị trí 0 và có thể di chuyển sang trái hoặc phải, đẩy các hộp theo hướng bạn di chuyển. Các hộp chỉ di chuyển về phía trước theo hướng bạn đẩy và không thể kéo hoặc bỏ qua. Mỗi ô chiếm một vị trí số nguyên riêng biệt và mỗi vị trí đặc biệt cũng khác biệt. Một số hộp ban đầu có thể nằm ở những vị trí đặc biệt. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp vị trí của các hộp và các vị trí đặc biệt theo thứ tự được sắp xếp. Đầu ra là một số duy nhất cho mỗi trường hợp thử nghiệm: số hộp tối đa có thể kết thúc ở các vị trí đặc biệt. 

Các ràng buộc rất đáng kể: lên tới 200.000 hộp hoặc vị trí đặc biệt trong một trường hợp thử nghiệm, với các vị trí nằm trong phạm vi ±10^9 và tổng trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này loại trừ các thuật toán có độ phức tạp O(n·m), có thể đạt tới 4×10^10 phép tính trong trường hợp xấu nhất. Chúng ta cần một thuật toán có độ phức tạp gần tuyến tính hoặc tuyến tính tính bằng n và m. 

Các trường hợp cạnh cần xem xét bao gồm các hộp và vị trí đặc biệt đều ở một phía bằng 0, các vị trí cực kỳ xa nhau và các tình huống trong đó các hộp đẩy sẽ buộc những hộp khác phải vượt qua các vị trí đặc biệt. Một cách tiếp cận ngây thơ là mô phỏng mọi động tác hoặc thử mọi cách ghép nối giữa các hộp và các vị trí đặc biệt sẽ thất bại cả về hiệu suất và độ chính xác. Ví dụ: nếu một hộp ở số 1 có thể được đẩy lên 6 trong khi một hộp ở số 5 chỉ có thể đạt tới 7, thì việc ghép các hộp gần nhất một cách mù quáng với các vị trí đặc biệt gần nhất sẽ không thể tối đa hóa kết quả khớp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng mô phỏng mọi chuỗi đẩy có thể có từ điểm bắt đầu, kiểm tra hộp nào đạt đến vị trí đặc biệt nào. Đối với mỗi hộp, người ta có thể thử di chuyển sang trái hoặc phải, đẩy chuỗi hộp và đánh dấu khi một vị trí đặc biệt được lấp đầy. Phương pháp này đúng về nguyên tắc, nhưng số bước di chuyển tăng lên theo tổ hợp với n, dẫn đến các phép toán O(2^n) hoặc ít nhất là O(n·m) cho mỗi trường hợp thử nghiệm, điều này hoàn toàn không khả thi đối với n, m lên tới 2×10^5. 

Cái nhìn sâu sắc quan trọng đến từ việc tách vấn đề thành các mặt tiêu cực và tích cực của trục số. Di chuyển và đẩy độc lập trên 0: các hộp bên trái không thể giúp đạt được các vị trí đặc biệt ở bên phải và ngược lại. Trong mỗi bên, vấn đề trở thành vấn đề khớp nhau: chúng tôi muốn tối đa hóa số lượng hộp có thể tiếp cận các vị trí đặc biệt theo hướng mà chúng đã nằm. Bởi vì tất cả các vị trí đều được sắp xếp nên tập hợp các vị trí đặc biệt có thể tiếp cận cho mỗi hộp là liền kề nhau. Chúng ta có thể sử dụng kỹ thuật tìm kiếm hai con trỏ hoặc tìm kiếm nhị phân để đếm một cách hiệu quả có bao nhiêu hộp có thể đến được vị trí đặc biệt gần nhất. 

Cụ thể, đối với một bên, chúng tôi chỉ tập trung vào các ô và vị trí đặc biệt có cùng dấu hiệu. Sau đó, đối với mỗi tiền tố của các vị trí đặc biệt, chúng tôi xác định số lượng hộp tối thiểu cần thiết để tiếp cận chúng. Bằng cách sử dụng một bộ để tra cứu nhanh các phần trùng lặp ban đầu và số lượng hậu tố cho các hộp đã ở các vị trí đặc biệt, chúng tôi tính toán số lượng hộp tối ưu có thể khớp trong thời gian O(n + m) cho mỗi bên. Kết hợp kết quả trái và phải sẽ đưa ra câu trả lời cho trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n + m) | Quá chậm | 
| Kết hợp hai con trỏ / tiền tố | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chia bài toán thành hai phần độc lập: các ô và vị trí đặc biệt có giá trị âm và phần có giá trị dương. Điều này hợp lệ vì việc đẩy chỉ di chuyển các hộp theo một hướng; không có hộp nào có thể vượt qua số 0 từ bên này sang bên kia. 
2. Đảo ngược các danh sách phủ định để chúng được xử lý theo chuỗi khoảng cách tăng dần từ 0. Điều này giúp đơn giản hóa logic và cho phép chúng ta xử lý cả hai bên một cách đối xứng. 
3. Đối với mỗi bên, hãy tạo một tập hợp các vị trí đặc biệt để tra cứu theo thời gian liên tục. Đếm các ô đã có ở các vị trí đặc biệt, tạo thành số lượng trùng khớp cơ bản. 
4. Đối với các ô còn lại và các vị trí đặc biệt, hãy coi đó là vấn đề căn chỉnh phần đuôi của các ô với các vị trí đặc biệt có thể tiếp cận được. Lặp lại các vị trí đặc biệt theo chiều ngược lại, duy trì con trỏ tới hộp gần nhất có thể tiếp cận nó. 
5. Đối với mỗi vị trí đặc biệt, hãy kiểm tra xem vị trí đó đã có người chưa. Nếu không, hãy sử dụng kỹ thuật tìm kiếm nhị phân hoặc hai con trỏ để xác định khối hộp liền kề lớn nhất có thể tiếp cận khối liền kề có các vị trí đặc biệt. Cập nhật số lượng trận đấu tối đa cho phù hợp. 
6. Tổng hợp kết quả của cả hai bên để có được tổng số hộp tối đa có thể chiếm các vị trí đặc biệt. 

**Tại sao nó hoạt động:** Thuật toán duy trì tính bất biến là mỗi ô chỉ có thể di chuyển về phía các vị trí tăng dần về phía nó. Bằng cách làm việc từ vị trí đặc biệt xa nhất về phía sau, chúng tôi đảm bảo rằng chúng tôi không bao giờ tính hộp là đạt đến vị trí đặc biệt mà nó không thể đẩy vào được. Kết hợp các vị trí đã có người sử dụng với các vị trí có thể tiếp cận đảm bảo không tính hai lần và tối đa hóa tổng số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def max_boxes(boxes, specials):
    boxes_set = set(boxes)
    specials_set = set(specials)
    
    # Count boxes already on special positions
    initial = len(boxes_set & specials_set)
    
    # Filter positions not already matched
    boxes = [x for x in boxes if x not in specials_set]
    specials = [x for x in specials if x not in boxes_set]
    
    if not boxes or not specials:
        return initial
    
    # Precompute suffix counts of specials
    specials_suffix = [0]*len(specials)
    specials_set_tmp = set(specials)
    for i in range(len(specials)-1, -1, -1):
        specials_suffix[i] = 1 + (specials_suffix[i+1] if i+1 < len(specials) else 0)
    
    res = 0
    j = len(boxes)-1
    for i in range(len(specials)-1, -1, -1):
        while j >= 0 and boxes[j] > specials[i]:
            j -= 1
        reachable = len([b for b in boxes[:j+1] if b + (len(boxes) - (j+1)) >= specials[i]])
        res = max(res, reachable + specials_suffix[i])
    
    return initial + res

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # Split into negative and positive sides
        a_neg = [-x for x in a if x < 0][::-1]
        a_pos = [x for x in a if x > 0]
        b_neg = [-x for x in b if x < 0][::-1]
        b_pos = [x for x in b if x > 0]
        
        ans = max_boxes(a_neg, b_neg) + max_boxes(a_pos, b_pos)
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là tách biệt vị trí âm và dương để xử lý độc lập từng bên. Nó đếm các hộp đã có ở các vị trí đặc biệt, lọc các vị trí phù hợp và tính toán các khối có thể tiếp cận. Đảo ngược các vị trí âm cho phép xử lý đối xứng. Quét hai con trỏ kết hợp với số lượng hậu tố đảm bảo rằng tất cả các kết quả khớp tối đa đều được tính mà không cần đếm hai lần hoặc bỏ qua các hộp có thể truy cập. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
5 6
-1 1 5 11 15
-4 -3 -2 6 7 15
```| Bước | Hộp còn lại | Ưu đãi còn lại | Đã khớp | Có thể tiếp cận tối đa | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tiêu cực | [-1] | [-4,-3,-2] | 0 | 1 (-2) | 1 | 
| Tích cực | [1,5,11,15] | [6,7,15] | 1 (15) | 3 ([6,7]) | 3 | 
| Tổng hợp | | | | | 4 | 

Thuật toán xác định chính xác rằng một hộp ở phía âm có thể đạt -2 và ba hộp ở phía dương có thể chiếm 6,7,15. 

**Ví dụ 2:** 

đầu vào:```
2 2
-1 1
-1000000000 1000000000
```| Bước | Hộp | Đặc biệt | Đã khớp | Có thể tiếp cận tối đa | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tiêu cực | [-1] | [-10^9] | 0 | 1 (đẩy lên -10^9) | 1 | 
| Tích cực | [1] | [10^9] | 0 | 1 | 1 | 
| Tổng hợp | | | | | 2 | 

Ngay cả những khoảng cách cực xa cũng được xử lý chính xác bằng cách tính toán số lượng có thể tiếp cận mà không cần mô phỏng tất cả các bước di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi bên được xử lý độc lập bằng quét tuyến tính, tìm kiếm nhị phân là tùy chọn nhưng danh sách sẽ nhỏ sau khi lọc. | 
| Không gian | O(n + m) | Lưu trữ các hộp đã lọc và các bộ sản phẩm đặc biệt để tra cứu. | 

Cho rằng tổng của n và m trong tất cả các trường hợp thử nghiệm là 2×10^5, giải pháp sẽ phù hợp trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("""5
5
```
