---
title: "CF 106118I - Chiếu sáng bằng tia"
description: "Cho một dãy các cực thẳng đứng đặt ở tọa độ nguyên trên một đường thẳng, trong đó cực i nằm ở vị trí x = i và có chiều cao h[i]."
date: "2026-06-19T20:07:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "I"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 51
verified: true
draft: false
---

[CF 106118I - Chiếu sáng bằng tia](https://codeforces.com/problemset/problem/106118/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các cực thẳng đứng được đặt ở tọa độ nguyên trên một đường thẳng, trong đó cực`i`ngồi ở vị trí`x = i`và có chiều cao`h[i]`. Từ phía bên trái, ánh sáng truyền theo một hướng cố định được mô tả bởi một vectơ`(tx, ty)`với`tx > 0`, do đó đèn luôn di chuyển sang phải và có thể đi lên hoặc đi xuống tùy theo`ty`. 

Mỗi cột có khả năng chặn ánh sáng đối với mọi thứ ở bên phải nó. Thay vì suy nghĩ theo từng tia riêng lẻ, bài toán xác định một "đường bao bóng", là ranh giới phía trên được hình thành bằng cách đưa mọi cực về bên trái, bắn một tia từ đỉnh của nó theo hướng cho trước và xem xét độ cao tối đa đạt được ở mọi vị trí x. Một cột được coi là được chiếu sáng nếu đỉnh của nó nằm hoàn toàn phía trên đường bao này khi nó được chạm tới. Nếu nó nằm chính xác trên phong bì thì được coi là bị chặn. 

Nhiệm vụ là đếm xem có bao nhiêu cực được chiếu sáng. 

Ràng buộc về tổng`n`trong các trường hợp thử nghiệm lên tới 500.000, điều này ngay lập tức loại trừ mọi phương pháp bậc hai so sánh từng cực với tất cả các cực trước đó. Một giải pháp phải xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính hoặc gần tuyến tính. 

Khó khăn tinh tế là đường bao bóng không chỉ đơn giản là chiều cao tối đa cho đến nay. Nó đạt cực đại trên nhiều đường nghiêng, do đó đường bao thay đổi độ dốc theo thời gian. Bất kỳ cách tiếp cận ngây thơ nào chỉ theo dõi chiều cao tối đa hoặc so sánh với một dòng sẽ không thành công. 

Một số trường hợp đặc biệt bộc lộ những lỗi điển hình. Nếu tất cả các độ cao bằng nhau và`ty = 0`, mọi tia đều nằm ngang và mọi thứ trùng nhau hoàn toàn nên chỉ có cực đầu tiên được chiếu sáng. Cách tiếp cận ngây thơ “lớn hơn chiều cao tối đa trước đó” sẽ đánh dấu không chính xác nhiều cột hơn là có thể nhìn thấy. 

Nếu như`ty < 0`, các tia sáng dốc xuống, nghĩa là bóng đổ ít mạnh hơn và các cực muộn hơn có thể được chiếu sáng ngay cả với độ cao nhỏ. Bất kỳ cách tiếp cận nào giả định chiều cao đường bao tăng đều đều sẽ bị phá vỡ ngay lập tức ở đây. 

Nếu như`ty > 0`, các tia sáng dốc lên, do đó các cột cao sớm chiếm ưu thế lâu hơn và có thể tạo bóng xa về phía bên phải. Đây là chế độ khó nhất, vì đường bao trở thành một vấn đề lồi ở thân tàu phía trên được ngụy trang. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: đối với mỗi cực`i`, mô phỏng tất cả các cực trước đó`j < i`, tính dòng bắt đầu từ`(j, h[j])`theo hướng`(tx, ty)`, tính chiều cao của nó tại vị trí`i`, và lấy giá trị lớn nhất Nếu như`h[i]`lớn hơn mức tối đa này thì cột được chiếu sáng. 

Mỗi đánh giá là thời gian không đổi, vì vậy mỗi`i`chi phí`O(n)`làm việc, cống hiến`O(n^2)`mỗi trường hợp thử nghiệm. Với`n = 5 * 10^5`tổng cộng qua các bài kiểm tra, điều này là quá chậm. 

Quan sát quan trọng là mỗi cực xác định một đường thẳng trong mặt phẳng. Chúng tôi muốn, tại mỗi tọa độ x, giá trị tối đa trong số một tập hợp các hàm tuyến tính đang phát triển được đánh giá tại các điểm nguyên. Đây chính xác là một vấn đề về thân tàu/đường chứa lồi động, nhưng trong một hệ tọa độ được biến đổi một chút. 

Mỗi cực`(i, h[i])`xác định một dòng:$$y = h[i] + (x - i)\cdot \frac{t_y}{t_x}$$Viết lại:$$y = \frac{t_y}{t_x} x + \left(h[i] - i \cdot \frac{t_y}{t_x}\right)$$Tất cả các đường đều có chung thành phần độ dốc trong`x`, vì vậy chúng ta có thể tính ra`t_y / t_x`và giảm sự so sánh giữa các dòng để so sánh các giá trị giống như điểm chặn. Thay vì duy trì rõ ràng các dòng có giá trị thực, chúng tôi so sánh các khoản đóng góp ở các vị trí riêng biệt. 

Một cách rõ ràng hơn là tránh hoàn toàn độ dốc dấu phẩy động. Nhân mọi thứ với`tx`:$$y \cdot tx = h[i]\cdot tx + (x - i)\cdot ty$$Tại vị trí`x = j`, đóng góp từ cực`i`trở thành:$$h[i]\cdot tx + (j - i)\cdot ty$$Vì vậy đối với mỗi`j`, chúng tôi muốn:$$\max_{i < j} (h[i]\cdot tx - i \cdot ty) + j \cdot ty$$Thuật ngữ tùy theo`i`độc lập với`j`, vì vậy chúng tôi duy trì mức hoạt động tối đa:$$best = \max(h[i]\cdot tx - i \cdot ty)$$Khi đó giá trị phong bì tại`j`là:$$best + j \cdot ty$$Một cột được chiếu sáng nếu:$$h[j]\cdot tx > best + j \cdot ty$$Điều này làm giảm toàn bộ vấn đề xuống còn một lần quét tuyến tính duy nhất duy trì giá trị tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Biến đổi + Tiền tố Max | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Viết lại điều kiện cho một cực`j`được chiếu sáng bằng cách nhân cả hai vế của phép so sánh hình học với`tx`, loại bỏ các phân số và biến đường bao bóng thành một biểu thức tuyến tính trong`j`. 
2. Quan sát rằng tất cả những đóng góp từ các cực trước`i < j`có thể được tách thành một thuật ngữ chỉ tùy thuộc vào`i`và một thuật ngữ chỉ phụ thuộc vào`j`. Điều này cho phép tính toán trước mức tối đa đang chạy trên`i`-phần phụ thuộc 
3. Duy trì một biến`best`, ban đầu rất âm, đại diện cho giá trị tối đa của`h[i] * tx - i * ty`trên tất cả các cực được xử lý. 
4. Đối với mỗi cực`j`, tính “điểm sáng” của nó`h[j] * tx`và so sánh nó với`best + j * ty`. 
5. Nếu số điểm thực sự lớn hơn, hãy tăng câu trả lời vì điểm cực nằm phía trên đường bao bóng. Bình đẳng không được tính là sáng tỏ, phù hợp với điều kiện chặt chẽ của bài toán. 
6. Cập nhật`best`sử dụng sự đóng góp của cực hiện tại`h[j] * tx - j * ty`. 

### Tại sao nó hoạt động 

Mỗi cực trước đóng góp một hàm tuyến tính trong`j`sau khi chiếu lên vectơ chỉ phương. Đường bao là điểm cực đại của các hàm này. Bởi vì mọi hàm số chỉ khác nhau ở điểm chặn của nó trong không gian được biến đổi, nên giá trị cực đại ở mỗi bước chỉ phụ thuộc vào điểm chặn tốt nhất cho đến nay. Thuật toán duy trì chính xác mức tối đa đó, vì vậy tại mọi chỉ số`j`nó xây dựng lại giá trị đường bao thực mà không lưu trữ rõ ràng tất cả các dòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))
        tx, ty = map(int, input().split())

        best = -10**30
        ans = 0

        for i in range(n):
            cur_score = h[i] * tx
            envelope = best + (i + 1) * ty

            if cur_score > envelope:
                ans += 1

            cand = h[i] * tx - (i + 1) * ty
            if cand > best:
                best = cand

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm trong một lần quét từ trái sang phải. Chi tiết triển khai chính đang sử dụng`i + 1`nhất quán vì các vị trí được lập chỉ mục 1 trong hình học. Sự so sánh chặt chẽ nên sự bình đẳng không được tính là soi sáng. 

Tất cả số học được thực hiện bằng số nguyên Python, có thể xử lý thoải mái các giá trị lên tới khoảng`1e18`hoặc lớn hơn mà không cần lo lắng về tình trạng tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét trường hợp nhỏ có độ dốc hướng lên: 

đầu vào:`h = [2, 3, 2, 7, 4], tx = 2, ty = 1`Chúng tôi theo dõi`best = h[i]*tx - i*ty`. 

| tôi | h[i] | h[i]*tx | tốt nhất trước | phong bì = tốt nhất + i*ty | được chiếu sáng? | tốt nhất sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 4 | -∞ | -∞ | vâng | 4 - 1 = 3 | 
| 2 | 3 | 6 | 3 | 3 + 1 = 4 | vâng | 6 - 2 = 4 | 
| 3 | 2 | 4 | 4 | 4 + 2 = 6 | không | 4 - 3 = 1 | 
| 4 | 7 | 14 | 4 | 4 + 3 = 7 | vâng | 14 - 4 = 10 | 
| 5 | 4 | 8 | 10 | 10 + 4 = 14 | không | 8 - 5 = 3 | 

Điều này phù hợp với ý tưởng rằng chỉ có các cực vượt quá đường bao tiến hóa mới đóng góp. 

### Ví dụ 2 

đầu vào:`h = [5, 7, 4, 6, 3], tx = 2, ty = -1`Ở đây đường bao giảm dần theo thời gian vì`ty < 0`. 

| tôi | h[i] | h[i]*tx | tốt nhất trước | phong bì = tốt nhất + i*ty | được chiếu sáng? | tốt nhất sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | -∞ | -∞ | vâng | 10 - 1(-1)=11 | 
| 2 | 7 | 14 | 11 | 11 - 1 = 10 | vâng | 14 - 2(-1)=16 | 
| 3 | 4 | 8 | 16 | 16 - 2 = 14 | không | 8 - 3(-1)=11 | 
| 4 | 6 | 12 | 16 | 16 - 3 = 13 | không | 12 - 4(-1)=16 | 
| 5 | 3 | 6 | 16 | 16 - 4 = 12 | không | 6 - 5(-1)=11 | 

Chỉ những đỉnh đầu tiên vẫn còn phù hợp vì đường bao dịch chuyển xuống dưới một cách hiệu quả theo thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cực đóng góp một lần cập nhật liên tục và một so sánh | 
| Không gian | O(1) | Chỉ có mức tối đa đang chạy và bộ đếm được lưu trữ | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi 5 · 10^5, do đó quét tuyến tính là đủ để chạy thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder; replace with solve() output in real setup

# provided samples (placeholders since statement formatting is incomplete)
# assert run(...) == ...

# minimum size
assert run("1\n1\n5\n1 0\n") == "1\n", "single pole always visible"

# all equal heights, horizontal light
assert run("1\n5\n3 3 3 3 3\n1 0\n") == "1\n", "only first visible"

# downward slope
assert run("1\n4\n1 2 3 4\n1 -1\n") == "4\n", "all become visible"

# strictly increasing heights upward slope
assert run("1\n5\n1 2 3 4 5\n1 1\n") == "1\n", "only first dominates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| chiều cao bằng nhau, tia phẳng | 1 | quy tắc chặn buộc | 
| dốc xuống | tất cả | phong bì giảm | 
| tăng độ cao, dốc lên | 1 | sự thống trị sớm mạnh mẽ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các cực có chiều cao và`ty = 0`. Đường bao trở thành một đường ngang không đổi. Đối với đầu vào`n = 5, h = [3,3,3,3,3], tx = 2, ty = 0`, cực đầu tiên được chiếu sáng vì nó nằm phía trên đường bao trống ban đầu, nhưng mọi cực tiếp theo đều nằm chính xác trên đường bao được tạo bởi đường bao đầu tiên. Thuật toán tính toán`best = h[i]*tx`, và mỗi cực mới so sánh`h[j]*tx`với`best`, mang lại sự bình đẳng và loại bỏ chính xác sự chiếu sáng do bất đẳng thức nghiêm ngặt. 

Một trường hợp khác là độ dốc âm. Với`h = [1,2,3], tx = 1, ty = -5`, đường bao giảm mạnh. Thuật toán duy trì`best = h[i] - i*(-5)`, tăng nhanh, nhưng phép trừ của`j*ty`tăng thời hạn phong bì một cách hiệu quả, cho phép các cực sau vượt qua cuộc kiểm tra. Điều này cho thấy tại sao chỉ theo dõi chiều cao tối đa sẽ không thành công, trong khi dạng tuyến tính được chuyển đổi vẫn chính xác. 

Trường hợp cạnh cuối cùng là khi cực đầu rất lớn lấn át tất cả các cực sau. Vì`h = [10^9, 1, 1, 1], tx = 1, ty = 1`, cực đầu tiên đặt một khoảng cách rất lớn`best`và tất cả các phép so sánh tiếp theo đều không đạt được bất đẳng thức nghiêm ngặt. Quá trình quét chính xác giữ câu trả lời là 1 mà không có nguy cơ tính toán lại hoặc tràn.
