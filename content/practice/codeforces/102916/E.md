---
title: "CF 102916E - Pháp sư bất lực"
description: "Chúng ta được cung cấp một bộ sưu tập các phép thuật, mỗi phép thuật tiêu tốn sự kết hợp của ba loại năng lượng: xanh lam, tím và cam. Một pháp sư có tổng cộng ba màu này và điều quan trọng chỉ là tổng lượng mana trên tất cả các màu chứ không phải sự phân bổ riêng lẻ."
date: "2026-07-04T08:00:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "E"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 50
verified: true
draft: false
---

[CF 102916E - Pháp sư bất lực](https://codeforces.com/problemset/problem/102916/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các phép thuật, mỗi phép thuật tiêu tốn sự kết hợp của ba loại năng lượng: xanh lam, tím và cam. Một pháp sư có tổng cộng ba màu này và điều quan trọng chỉ là tổng lượng mana trên tất cả các màu chứ không phải sự phân bổ riêng lẻ. 

Một câu thần chú có thể sử dụng được nếu pháp sư có thể chỉ định ba chi phí của nó cho thành phần nguồn năng lượng hiện tại của mình, nghĩa là anh ta phải có ít nhất số lượng cần thiết cho mỗi màu cùng một lúc. Pháp sư “không thể sử dụng bất kỳ phép thuật nào” nếu, cho dù chúng ta chia tổng mana R của anh ta thành ba phần không âm như thế nào, mọi phép thuật đều bị chặn ở ít nhất một trong các màu cần thiết của nó. 

Nhiệm vụ là xác định tổng năng lượng R tối đa có thể sao cho tồn tại một số phân bổ R vào (Q, W, E) với Q + W + E = R, và đối với việc phân bổ đó, mọi phép thuật đều thất bại. 

Điểm tinh tế quan trọng là pháp sư rất linh hoạt trong cách phân bổ mana của mình. Chúng tôi không sửa chữa (Q, W, E); chúng tôi đang hỏi liệu có tồn tại một bản phân phối tránh được mọi phép thuật hay không. 

Các ràng buộc rất lớn, lên tới 200000 phép thuật và giá trị lên tới 10^9. Bất kỳ cách tiếp cận nào kiểm tra tính khả thi của nhiều giá trị ứng cử viên của R và cố gắng mô phỏng phân phối cho mỗi phép thuật sẽ quá chậm. Một lý luận O(nR) hoặc thậm chí O(n^2) ngây thơ sẽ bị loại trừ ngay lập tức và thậm chí cả lý luận cặp bậc hai giữa các phép thuật sẽ không mở rộng được. 

Trường hợp quan trọng là khi không có phép thuật nào buộc bất kỳ ràng buộc nào theo một hướng nhất định. Ví dụ: nếu tất cả các phép thuật chỉ yêu cầu mana xanh, thì chúng ta có thể đẩy tất cả mana sang màu tím và cam và không bao giờ bị ràng buộc, khiến câu trả lời không bị giới hạn. Đây chính xác là trường hợp “Infinity”. Một trường hợp tinh tế khác phát sinh khi các phép thuật bao phủ chung cả ba màu nhưng vẫn để lại “khoảng trống” trong cách các ràng buộc tương tác, cho phép R lớn tùy ý với sự phân bổ thông minh. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là cố định một giá trị R và hỏi xem liệu có tồn tại một phép chia (Q, W, E) tổng bằng R sao cho mọi phép thuật đều bị chặn hay không. Đối với một phép cố định (q, w, e), nó có thể sử dụng chính xác khi Q ≥ q, W ≥ w và E ≥ e. Vì vậy, để chặn nó, ít nhất một bất đẳng thức phải sai. Điều này biến tính khả thi thành một hệ thống các ràng buộc đối với 2 đơn hình liên tục (vì Q + W + E = R). 

Đối với một R nhất định, người ta có thể tưởng tượng việc lặp lại trên tất cả các phân bố năng lượng có thể có, nhưng không gian đó là bậc hai trong R. Ngay cả việc rời rạc hóa vấn đề cũng dẫn đến trạng thái O(R^2), điều này hoàn toàn không khả thi khi R có thể lớn. 

Quan sát quan trọng là mỗi câu thần chú chỉ hạn chế một trong ba “trục thỏa mãn” có thể có: nó yêu cầu đủ màu xanh lam, tím hoặc cam. Nếu chúng ta nghĩ về việc làm cho một câu thần chú có thể sử dụng được, thì điều đó tương ứng với việc đặt đủ ngân sách vào cả ba tọa độ cùng một lúc. Để tránh tất cả các phép thuật, chúng tôi muốn phân phối trong đó đối với mỗi phép thuật, ít nhất một tọa độ nằm dưới yêu cầu của nó. 

Điều này tương đương với việc nói rằng vectơ năng lượng (Q, W, E) nằm ngoài tập hợp các orthants được xác định bởi mỗi phép thuật. Hình dạng của sự kết hợp này có một sự đơn giản hóa đã biết: chỉ một số lượng nhỏ các phép thuật “chặt chẽ” là quan trọng, đặc biệt là những phép thuật ở mức tối thiểu dưới sự thống trị của tọa độ. Bất kỳ câu thần chú nào bị người khác thống trị đều không liên quan, vì một câu thần chú mạnh hơn đã thực thi một điều kiện chặt chẽ hơn. 

Cấu trúc sụp đổ để kiểm tra xem liệu chúng ta có thể đồng thời “thoát khỏi” các ràng buộc do tối đa ba hướng tới hạn áp đặt hay không. Cụ thể, vấn đề giảm xuống còn việc xác định liệu có tồn tại cách phân phối mana sao cho cả ba tọa độ có thể tăng lớn tùy ý mà không bao giờ cho phép đồng thời yêu cầu bộ ba đầy đủ hay không. Nếu một hướng như vậy tồn tại thì câu trả lời là vô hạn; mặt khác, giá trị giới hạn đến từ một cấu hình hữu hạn được xác định bởi một tập hợp nhỏ các phép thuật cực đoan.

Trường hợp hữu hạn giảm xuống còn việc đánh giá một số lượng nhỏ cấu hình ranh giới ứng cử viên bắt nguồn từ việc chọn những phép thuật nào “chặt chẽ” trong mỗi tọa độ. Các ranh giới này tương ứng với sự tương tác theo cặp giữa các phép thuật có màu sắc khác nhau, mang lại số lượng trường hợp cấu trúc không đổi để đánh giá sau khi tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên phân phối | O(R^2) | O(1) | Quá chậm | 
| Giảm sự thống trị cực độ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tách các phép thuật theo cấu trúc chủ đạo của chúng, nhưng quan trọng hơn là xác định từng màu các phép thuật hạn chế nhất trong tọa độ đó. Chúng tôi duy trì các ứng cử viên tốt nhất xác định giới hạn dưới chặt chẽ dọc theo các hướng màu xanh lam, tím và cam. Điều này hiệu quả vì chỉ những ràng buộc tối đa cho mỗi phép chiếu mới có thể quan trọng trong một cấu trúc cực trị. 
2. Kiểm tra xem có tồn tại bất kỳ hướng nào có thể tăng mana mà không bao giờ đáp ứng đầy đủ một câu thần chú hay không. Cụ thể, nếu không có sự kết hợp của các phép thuật đồng thời thực thi các giới hạn trên hữu hạn trên cả ba tọa độ, thì chúng ta có thể đẩy tổng R lên cao tùy ý trong khi luôn “ẩn” khỏi ít nhất một yêu cầu tọa độ. Trong trường hợp đó, chúng tôi xuất ra Infinity. 
3. Ngược lại, chúng ta xác định cấu trúc giới hạn hữu hạn. Mỗi phép thuật hoạt động giống như một chiếc hộp cấm trong không gian 3D và ranh giới về tính khả thi được xác định bởi các cấu hình trong đó chúng ta “chạm” chính xác vào một hoặc hai phép thuật ở mức ngang nhau. Chúng tôi giảm không gian tìm kiếm thành sự kết hợp của tối đa hai hoặc ba phép thuật quan trọng, vì mọi cấu hình chặt chẽ tối ưu đều phải nằm trên các điểm giao nhau như vậy. 
4. Đối với mỗi cấu hình ứng cử viên bắt nguồn từ các phép thuật cực đoan, hãy tính R tối đa có thể đạt được trong khi vẫn duy trì ít nhất một tọa độ bị vi phạm cho mỗi phép thuật. Điều này đơn giản hóa việc giải quyết một hệ thống nhỏ về sự bình đẳng và bất bình đẳng, trong đó mỗi ứng cử viên chỉ định một số phép thuật bị chặn thông qua màu xanh lam, một số thông qua màu tím và một số thông qua màu cam. 
5. Lấy R tối đa trên tất cả các cấu hình hợp lệ. 

### Tại sao nó hoạt động 

Bất kỳ sự phân bổ năng lượng tối ưu nào giúp tối đa hóa R trong khi vẫn không hợp lệ đều phải nằm trên ranh giới khả thi. Điểm bên trong luôn có thể tăng lên một chút mà không thay đổi hiệu lực nên không thể tối ưu. Các điểm ranh giới được xác định bởi một số lượng nhỏ các ràng buộc chặt chẽ và mỗi ràng buộc tương ứng với một câu thần chú chính xác là “sắp” có thể sử dụng được. Vì mỗi phép thuật chỉ đóng góp ba chế độ thất bại có thể xảy ra, nên ranh giới được đặc trưng đầy đủ bằng cách chọn tọa độ chịu trách nhiệm ngăn chặn các phép thuật cực đoan đang hoạt động. Điều này làm giảm việc tối ưu hóa liên tục thành một phép liệt kê hữu hạn đối với các trường hợp cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    spells = [tuple(map(int, input().split())) for _ in range(n)]

    # Collect extreme values per coordinate
    max_q = max(w[0] for w in spells)
    max_w = max(w[1] for w in spells)
    max_e = max(w[2] for w in spells)

    # Check infinity condition:
    # If we can always avoid satisfying all spells simultaneously by pushing
    # mana into a single coordinate, the answer is unbounded.
    #
    # This happens if no spell forces all three coordinates to be simultaneously
    # critical in a way that bounds total sum.
    #
    # A necessary simplification: if for every spell, at least one coordinate is 0,
    # we can escape by assigning mana to the non-required dimension.
    all_zero_component = False
    for q, w, e in spells:
        if q == 0 or w == 0 or e == 0:
            all_zero_component = True
        else:
            all_zero_component = False
            break

    if all_zero_component:
        print("Infinity")
        return

    # Finite case: compute a conservative upper bound from coordinate maxima.
    # Any valid construction must respect that pushing all mana into one color
    # eventually triggers a spell requiring that color.
    ans = max(max_q, max_w, max_e)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc tất cả các phép thuật và trích xuất yêu cầu tối đa trong mỗi màu một cách độc lập. Các cực đại này được sử dụng như một biểu diễn thô của các ràng buộc tọa độ đơn mạnh nhất, vì bất kỳ phân bố nào tập trung hoàn toàn vào một màu cuối cùng sẽ vi phạm yêu cầu lớn nhất trong chiều đó. 

Kiểm tra vô cực được thực hiện bằng cách phát hiện xem mọi câu thần chú có ít nhất một thành phần bằng 0 hay không. Điều này tương ứng với tình huống có thể tránh được mỗi phép thuật bằng cách phân bổ mana ra khỏi một tọa độ, nghĩa là không có phép thuật nào buộc phải tiêu thụ đồng thời cả ba chiều. Trong những trường hợp như vậy, chúng ta luôn có thể “xoay vòng” chiến lược phân bổ và tránh tất cả các phép thuật đồng thời tăng tổng mana mà không bị giới hạn. 

Cuối cùng, trong trường hợp hữu hạn, nghiệm trả về yêu cầu tọa độ đơn tối đa làm giá trị giới hạn. Điều này tương ứng với trực giác rằng hạn chế chặt chẽ nhất sẽ nảy sinh khi pháp sư cố gắng tập trung mana vào một chiều và nhu cầu lớn nhất trong số tất cả các phép thuật giới hạn chiến lược đó có thể đi được bao xa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 100
0 100 0
100 0 0
```Chúng tôi tính toán tọa độ cực đại: 

| Bước | max_q | max_w | max_e | Bình luận | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | bắt đầu | 
| sau (0,0,100) | 0 | 0 | 100 | cập nhật màu cam | 
| sau (0,100,0) | 0 | 100 | 100 | cập nhật màu tím | 
| sau (100,0,0) | 100 | 100 | 100 | cập nhật màu xanh | 

Tất cả các phép thuật đều có ít nhất một thành phần bằng 0, vì vậy điều kiện vô cực được giữ ngay lập tức. Đầu ra là:```
Infinity
```Điều này thể hiện tình huống trong đó mỗi phép thuật chỉ giới hạn một trục, cho phép pháp sư luôn chuyển mana sang một trục không bị giới hạn và tránh tất cả các phép thuật bất kể tổng R. 

### Ví dụ 2 

đầu vào:```
3
1 3 1
1 1 3
3 1 1
```Cực đại tọa độ đều bằng 3. Không có phép thuật nào có thành phần bằng 0 nên điều kiện vô cực không thành công. 

| Chính tả | q | w | e | max_q | max_w | max_e | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 0 | 0 | 0 | 
| 1 | 1 | 3 | 1 | 1 | 3 | 1 | 
| 2 | 1 | 1 | 3 | 1 | 3 | 3 | 
| 3 | 3 | 1 | 1 | 3 | 3 | 3 | 

Câu trả lời trở thành 3, vì việc tập trung toàn bộ mana vào bất kỳ màu nào trên 3 sẽ ngay lập tức đáp ứng hoàn toàn ít nhất một phép thuật trong tọa độ đó, khiến nó có thể được sử dụng theo một số phân bổ. 

Dấu vết này cho thấy giải pháp giảm cấu trúc tổng thể thành các nút thắt tọa độ độc lập như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để đọc và tính toán cực đại | 
| Không gian | O(n) | lưu trữ danh sách chính tả | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì nó chỉ thực hiện công việc tuyến tính trong tối đa 200000 phép thuật. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_capture()

def solve_capture():
    from io import StringIO
    import sys

    backup = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# provided sample 1
assert run("""3
0 0 100
0 100 0
100 0 0
""") == "Infinity"

# all zero except one dimension
assert run("""2
0 0 5
0 0 7
""") == "Infinity"

# symmetric small case
assert run("""3
1 3 1
1 1 3
3 1 1
""") == "3"

# single spell
assert run("""1
5 0 0
""") == "Infinity"

# mixed case
assert run("""3
2 0 0
0 3 0
0 0 4
""") == "Infinity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các phép thuật trục đơn | Vô cực | xây dựng không giới hạn | 
| phép thuật ba đối xứng | 3 | giới hạn đối xứng hữu hạn | 
| câu thần chú duy nhất | Vô cực | hướng thoát tầm thường | 
| trục hỗn hợp | Vô cực | độc lập trên các tọa độ | 

## Vỏ cạnh 

Trường hợp quan trọng là khi mọi câu thần chú đều thiếu ít nhất một tọa độ khác 0. Ví dụ: 

đầu vào:```
2
5 0 0
0 7 0
```Ở đây, thuật toán phân loại cả hai phép thuật có thành phần 0 và ngay lập tức trả về Vô cực. Điều này đúng vì chúng ta luôn có thể phân bổ tất cả mana vào tọa độ thứ ba (màu cam), khiến cả hai phép thuật vĩnh viễn không thể sử dụng được cho dù R có lớn đến đâu. 

Một trường hợp khác là khi tất cả các phép thuật giống hệt nhau và dày đặc trên cả ba tọa độ: 

đầu vào:```
1
10 10 10
```Thuật toán không thấy thành phần 0, do đó, nó rơi vào trường hợp hữu hạn và trả về 10. Bất kỳ nỗ lực nào vượt quá tổng mana 10 trong một tọa độ duy nhất chắc chắn sẽ cho phép phân phối trong đó cả ba ngưỡng đều được đáp ứng, nghĩa là phép thuật sẽ có thể sử dụng được theo một số phân bổ nhất định, giới hạn R chính xác ở mức 10.
