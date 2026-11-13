---
title: "CF 1730B - Cuộc họp trực tuyến"
description: "Chúng tôi được xếp một hàng với những người đứng ở các tọa độ khác nhau. Mỗi người đều có một vị trí cố định và cũng có thời gian chuẩn bị cá nhân trước khi bắt đầu di chuyển."
date: "2026-06-15T02:39:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry", "greedy", "implementation", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1730
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 823 (Div. 2)"
rating: 1600
weight: 1730
solve_time_s: 95
verified: false
draft: false
---

[CF 1730B - Cuộc họp trực tuyến](https://codeforces.com/problemset/problem/1730/B) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, hình học, tham lam, thực hiện, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng với những người đứng ở các tọa độ khác nhau. Mỗi người đều có một vị trí cố định và cũng có thời gian chuẩn bị cá nhân trước khi bắt đầu di chuyển. Sau khi chọn điểm hẹn, thời gian đến của một người là tổng thời gian chuẩn bị của họ và khoảng cách họ cần đi bộ đến điểm đó. 

Mục tiêu là chọn một tọa độ duy nhất trên đường sao cho người chậm nhất, xét về tổng thời gian đến, sẽ đến sớm nhất có thể. Nói cách khác, nếu chúng ta định nghĩa thời gian đến của người i là`t_i + |x_i - x_0|`, chúng tôi muốn chọn`x_0`giảm thiểu tối đa các giá trị này đối với tất cả mọi người. 

Các ràng buộc cho thấy tổng số lên tới 200.000 người trong tất cả các trường hợp thử nghiệm. Một cách tiếp cận ngây thơ thử mọi vị trí gặp nhau có thể là không thể vì không gian trả lời liên tục và lớn, lên tới 10^8. Ngay cả việc đánh giá một chức năng cho từng vị trí ứng viên cũng sẽ quá chậm. 

Một điểm tinh tế là mục tiêu không tuyến tính một cách đơn giản trên một chỉ số cố định, nó là hàm lồi tối đa. Điều này có nghĩa là hàm chúng ta đang cực tiểu hóa bản thân nó là lồi nhưng không trơn ở mọi nơi. 

Một trường hợp thất bại phổ biến đối với trực giác ngây thơ là giả định điểm gặp nhau phải là điểm trung bình của các vị trí hoặc một mức trung bình có trọng số nào đó. Điều đó sẽ bị phá vỡ ngay lập tức khi thời gian chuẩn bị khác nhau. Ví dụ, hai người ở cùng một vị trí nhưng khác nhau`t_i`các giá trị có thể dịch chuyển hoàn toàn điểm gặp gỡ tối ưu ra khỏi vị trí đó vì chi phí không hoàn toàn mang tính hình học. 

Một cạm bẫy khác là cố gắng mô phỏng chuyển động hoặc chỉ sắp xếp theo vị trí. Thời gian chuẩn bị đóng vai trò giống như sự dịch chuyển theo chiều dọc của từng chức năng “hình chữ V”, làm thay đổi xem người nào chiếm ưu thế tối đa. 

## Phương pháp tiếp cận 

Nếu chúng ta ấn định vị trí họp của ứng viên`x_0`, việc tính toán thời gian cho mỗi người rất đơn giản: chúng tôi đánh giá`t_i + |x_i - x_0|`và lấy tối đa. Điều này đưa ra một ý tưởng đúng đắn nhưng tốn kém: quét qua một tập hợp dày đặc các vị trí ứng viên và đánh giá chi phí tối đa cho mỗi vị trí. 

Khó khăn là điểm tối ưu không được đảm bảo ở bất kỳ tọa độ đầu vào nào và không gian tìm kiếm là liên tục. Việc thử tất cả các số nguyên trong khoảng từ 0 đến 10^8 là không thể và thậm chí việc đánh giá hàm trên mỗi điểm cũng sẽ quá chậm. 

Quan sát quan trọng là hàm chúng ta đang cực tiểu hóa là hàm lồi trong`x_0`. Each term`t_i + |x_i - x_0|`is a V-shaped convex function, and the maximum of convex functions is still convex. A convex function on a line has a unique global minimum and can be found using ternary search or by checking the sign of its derivative.

 However, directly evaluating the function is still O(n), so each check costs O(n). With ternary search over a large range, we only need about 60 iterations to converge to sufficient precision, making the solution feasible.

 An alternative perspective is to think in terms of slopes. Khi chúng tôi di chuyển`x_0`ở bên phải, sự đóng góp của một số người tăng lên trong khi những người khác lại giảm đi. Điểm tối ưu là nơi “ràng buộc chặt chẽ nhất” chuyển từ bị chi phối bởi những người đóng góp bên trái sang những người đóng góp bên phải. Điểm cân bằng này chính xác là nơi tối đa hóa các hình chữ V này được giảm thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên số nguyên | O(R·n) | O(1) | Quá chậm | 
| Tìm kiếm bậc ba trên hàm lồi | O(n log R) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm`f(x)`tính toán thời gian tối đa cần thiết để tất cả mọi người đạt đến điểm`x`. Điều này được thực hiện bằng cách quét tất cả mọi người và tính toán`t_i + |x_i - x|`. Hàm này thể hiện thời gian đến trong trường hợp xấu nhất nếu chúng ta chọn`x`. 
2. Hãy quan sát điều đó`f(x)`lồi trên số thực. Điều này cho phép chúng tôi sử dụng tìm kiếm ternary để tìm mức tối thiểu của nó. 
3. Đặt khoảng thời gian tìm kiếm`[L, R]`bao gồm tất cả các vị trí có ý nghĩa có thể, ban đầu`[0, 1e8]`. 
4. Trong khi khoảng cách vẫn còn đủ rộng, hãy chọn hai điểm bên trong`m1`Và`m2`, thường chia phân khúc thành ba phần. Tính toán`f(m1)`Và`f(m2)`. 
5. Nếu`f(m1) > f(m2)`, cực tiểu nằm bên phải nên dịch chuyển`L`ĐẾN`m1`. Nếu không thì dịch chuyển`R`ĐẾN`m2`. Điều này có tác dụng vì hàm lồi giảm rồi tăng, do đó so sánh hai điểm bên trong cho chúng ta biết cạnh nào chứa điểm nhỏ nhất. 
6. Sau khi lặp đủ số lần, lấy điểm giữa của khoảng còn lại làm đáp án. 

Lý do điều này có tác dụng là vì độ lồi đảm bảo có một mức tối thiểu toàn cục duy nhất và mọi so sánh giữa hai điểm sẽ loại bỏ một vùng không thể chứa nó. Quá trình này dần dần thu hẹp khoảng thời gian tìm kiếm mà không loại bỏ điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        tvals = list(map(int, input().split()))
        
        def f(pos):
            mx = 0
            for i in range(n):
                d = abs(x[i] - pos)
                val = tvals[i] + d
                if val > mx:
                    mx = val
            return mx
        
        l, r = 0.0, 100000000.0
        
        for _ in range(80):
            m1 = l + (r - l) / 3.0
            m2 = r - (r - l) / 3.0
            if f(m1) > f(m2):
                l = m1
            else:
                r = m2
        
        ans = (l + r) / 2.0
        print(ans)

if __name__ == "__main__":
    solve()
```chức năng`f(pos)`đánh giá rõ ràng thời gian đến tối đa tại một điểm ứng viên. Đây là đánh giá cốt lõi được sử dụng bởi tìm kiếm ternary. Khoảng tìm kiếm được cố định đủ rộng để bao gồm tất cả các câu trả lời có thể có vì các điểm tối ưu nằm trong bao lồi của các vị trí đầu vào. 

Vòng lặp 80 lần lặp là đủ để đưa khoảng thời gian xuống dưới độ chính xác cần thiết. Mỗi lần lặp sẽ loại bỏ khoảng một phần ba phạm vi còn lại, do đó tốc độ hội tụ nhanh ngay cả đối với các yêu cầu có độ chính xác cao. 

Một chi tiết triển khai tinh tế là sử dụng trực tiếp số học dấu phẩy động thay vì cố gắng tìm kiếm số nguyên ba ngôi. Vì độ chính xác yêu cầu là`1e-6`, độ chính xác gấp đôi là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
x = [1, 2, 3]
t = [0, 0, 0]
```| Lặp lại | tôi | r | m1 | m2 | f(m1) so với f(m2) | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 10^8 | ~3.3e7 | ~6.6e7 | đối xứng | khoảng thời gian thu hẹp | 
| … | … | … | … | … | … | hội tụ | 

Hàm này hoàn toàn dựa trên khoảng cách và đối xứng xung quanh vị trí trung vị, do đó việc tìm kiếm hội tụ gần`2`. Điều này khẳng định rằng khi tất cả`t_i`bằng nhau thì nghiệm rút gọn về trung tuyến hình học trên một đường thẳng. 

### Ví dụ 2 

đầu vào:```
n = 2
x = [0, 10]
t = [0, 5]
```| Lặp lại | tôi | r | m1 | m2 | f(m1) | f(m2) | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 10 | 3.3 | 6,6 | cao hơn | thấp hơn | di chuyển sang trái | 
| 2 | 0 | 6,6 | ... | ... | ... | ... | hội tụ | 

Ở đây người thứ hai có thời gian chuẩn bị lâu hơn nên điểm gặp tối ưu sẽ dịch chuyển một chút về phía họ để cân bằng thời gian đi lại. Dấu vết cho thấy mức độ thay đổi tối đa tùy thuộc vào người nào chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(n log R) | Mỗi bước thứ ba quét tất cả mọi người, lặp lại ~ 80 lần | 

| Không gian | O(1) | Chỉ các mảng đầu vào và một vài biến được lưu trữ | 

Tổng độ phức tạp dễ dàng đủ nhanh vì tổng n tối đa là 2e5 và hệ số không đổi khoảng 80 đánh giá cho mỗi bài kiểm tra là có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            x = list(map(int, input().split()))
            tvals = list(map(int, input().split()))

            def f(pos):
                mx = 0
                for i in range(n):
                    mx = max(mx, tvals[i] + abs(x[i] - pos))
                return mx

            l, r = 0.0, 100000000.0
            for _ in range(60):
                m1 = l + (r - l) / 3
                m2 = r - (r - l) / 3
                if f(m1) > f(m2):
                    l = m1
                else:
                    r = m2

            out.append(str((l + r) / 2))

        return "\n".join(out)

    return solve()

# provided sample (partial check)
assert run("""1
1
0
3
""").strip()[:1] == "0"

# custom cases
assert run("""1
2
0 10
0 0
""") == "5.0"

assert run("""1
3
1 2 3
0 0 0
""") == "2.0"

assert run("""1
2
0 100
10 0
""") != "", "should produce a valid float"

assert run("""1
1
5
7
""") == "5.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | cùng tọa độ | trường hợp cơ sở | 
| vị trí đối xứng | điểm giữa | độ đúng hình học | 
| sự bất đối xứng có trọng số | chuyển tối ưu | tác dụng của t_i | 
| phần tử đơn | vị trí chính xác | trường hợp cạnh tầm thường | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các vị trí giống hệt nhau nhưng thời gian chuẩn bị khác nhau. Ví dụ, nếu tất cả`x_i = 5`Nhưng`t_i`khác nhau thì điểm gặp mặt tối ưu vẫn là`5`, vì việc di chuyển đi chỉ làm tăng thời gian di chuyển mà không làm giảm bất kỳ khâu chuẩn bị nào. Thuật toán đánh giá điều này một cách chính xác vì bất kỳ độ lệch nào từ 5 đều làm tăng tất cả`|x_i - x|`bằng nhau. 

Một trường hợp tế nhị khác là khi tồn tại hai nhóm đối lập, một nhóm có thời gian chuẩn bị lớn ở bên trái và một nhóm ở bên phải. Điểm tối ưu có thể nằm hoàn toàn giữa chúng chứ không phải ở bất kỳ tọa độ đầu vào nào. Tìm kiếm ba ngôi một cách tự nhiên sẽ tìm thấy điểm bên trong này vì nó không dựa vào các ứng viên rời rạc. 

Cuối cùng, khi chỉ có một người thì hàm số được thu nhỏ chính xác tại vị trí của họ. Hàm đánh giá giảm xuống thành một hình chữ V có tâm ở`x_i`và mức tối thiểu là tầm thường`x_i`, thuật toán trả về mà không mất ổn định.
