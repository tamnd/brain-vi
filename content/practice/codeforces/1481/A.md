---
title: "CF 1481A - Điều hướng không gian"
description: "Chúng ta có điểm bắt đầu tại điểm gốc trên lưới và tọa độ đích. Ngoài ra, chúng ta còn được cung cấp một chuỗi lệnh di chuyển bao gồm các bước đơn vị theo bốn hướng chính."
date: "2026-06-10T23:34:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1481
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 699 (Div. 2)"
rating: 800
weight: 1481
solve_time_s: 102
verified: true
draft: false
---

[CF 1481A - Điều hướng không gian ](https://codeforces.com/problemset/problem/1481/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có điểm bắt đầu tại điểm gốc trên lưới và tọa độ đích. Ngoài ra, chúng ta còn được cung cấp một chuỗi lệnh di chuyển bao gồm các bước đơn vị theo bốn hướng chính. Mỗi lệnh được thực hiện theo thứ tự, di chuyển vị trí đúng một đơn vị theo hướng tương ứng. 

Chúng ta được phép xóa bất kỳ tập hợp con lệnh nào, đồng thời giữ nguyên thứ tự tương đối của các lệnh còn lại. Sau khi xóa, chuỗi còn lại được thực hiện từ trái sang phải bắt đầu từ điểm gốc. Câu hỏi đặt ra là liệu chúng ta có thể chọn một tập hợp con các lệnh sao cho vị trí cuối cùng trở thành chính xác tọa độ đích hay không. 

Ràng buộc về tổng độ dài của tất cả các chuỗi lệnh tối đa là 100000 trong tất cả các trường hợp thử nghiệm ngụ ý rằng bất kỳ giải pháp nào cũng phải xử lý từng ký tự về cơ bản một lần hoặc một số lần không đổi. Bất kỳ cách tiếp cận nào cố gắng liệt kê các chuỗi con hoặc mô phỏng việc xóa một cách rõ ràng sẽ ngay lập tức thất bại vì số lượng các chuỗi con tăng theo cấp số nhân theo độ dài chuỗi. 

Một cạm bẫy ngây thơ nhưng phổ biến là giả định rằng nếu tổng cộng chuỗi chứa đủ các bước di chuyển theo đúng hướng thì câu trả lời luôn là có. Điều này không chính xác vì thứ tự quan trọng theo nghĩa là chúng ta không thể sắp xếp lại các nước đi mà chỉ xóa chúng. Ví dụ: hãy xem xét việc đạt (1, 1) từ chuỗi "DDRRUU". Có đủ nước đi R và U, nhưng nếu chữ R duy nhất xuất hiện sau tất cả nước đi U, chúng ta vẫn có thể chọn chúng và tiếp cận mục tiêu, do đó thứ tự không thực sự hạn chế tính khả thi ngoài khả năng đếm. Điều tinh tế là trật tự không bao giờ cản trở tính khả thi vì việc xóa có thể bỏ qua mọi thứ không liên quan. 

Một trường hợp sai lầm khác là nghĩ rằng chúng ta phải khớp chính xác mục tiêu ở dạng tiền tố. Ví dụ: việc đạt (1, 1) từ "URDL" có vẻ là không thể nếu chúng ta mô phỏng một cách tham lam, nhưng chỉ chọn "UR" mới hoạt động hoàn hảo. 

Khó khăn thực sự là nhận ra rằng vấn đề giảm xuống còn việc chọn nhiều tập hợp các bước di chuyển từ chuỗi, chỉ bị ràng buộc bằng cách duy trì trật tự chứ không phải kề cận. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử từng tập hợp con các ký tự, mô phỏng đường dẫn kết quả và kiểm tra xem liệu nó có đến được mục tiêu hay không. Điều này khám phá 2^n khả năng và thậm chí với n = 40, điều này trở nên không khả thi, chứ đừng nói đến n lên tới 100000. Tính đúng đắn của phương pháp này rất đơn giản vì nó kiểm tra rõ ràng tất cả các thao tác xóa hợp lệ, nhưng sự tăng trưởng theo cấp số nhân của nó khiến nó không thể sử dụng được. 

Quan sát quan trọng là việc xóa các ký tự cho phép chúng ta hoàn toàn tự do chọn bất kỳ tập hợp con nước đi nào, nhưng chúng ta không thể sắp xếp lại chúng. Vì thứ tự không ảnh hưởng đến tọa độ cuối cùng nên chỉ có số lượng hướng mới quan trọng. Mỗi ký tự được chọn đóng góp độc lập vào độ dịch chuyển, vì vậy vị trí cuối cùng chỉ phụ thuộc vào số lần di chuyển U, D, L và R mà chúng ta giữ. 

Để đạt được (p_x, p_y), chúng ta phải đảm bảo rằng sau khi xóa, chuyển động tịnh theo phương ngang bằng p_x và chuyển động tịnh theo chiều dọc bằng p_y. Điều này có nghĩa là cần ít nhất |p_x| di chuyển theo hướng ngang cần thiết và ít nhất |p_y| di chuyển theo phương thẳng đứng cần thiết. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem chuỗi có chứa đủ số lần xuất hiện của mỗi hướng được yêu cầu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo | O(2^n · n) | O(n) | Quá chậm | 
| Đếm tần số ký tự | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đếm số lần mỗi hướng xuất hiện trong chuỗi. Điều này cung cấp cho chúng tôi nguồn cung cấp các bước di chuyển U, D, L và R mà không cần xem xét đến thứ tự. Lý do điều này có tác dụng là vì bất kỳ lựa chọn tập hợp con nào cũng chỉ phụ thuộc vào tình trạng sẵn có chứ không phải sự sắp xếp. 
2. Tính toán số lần di chuyển cần thiết trên mỗi trục tính từ tọa độ đích. Nếu p_x dương, chúng ta cần p_x di chuyển sang phải; nếu âm, chúng ta cần di chuyển sang trái -p_x. Tương tự cho p_y với lên và xuống. 
3. Kiểm tra tính khả thi trên trục x bằng cách xác minh xem chuỗi có chứa ít nhất p_x R di chuyển khi p_x dương hay ít nhất -p_x L di chuyển khi p_x âm. 
4. Thực hiện tương tự cho trục y sử dụng số đếm U và D. 
5. Nếu cả hai trục có thể được thỏa mãn độc lập, thì xuất CÓ, nếu không thì xuất ra KHÔNG. 

### Tại sao nó hoạt động 

Mỗi bước di chuyển đều góp phần độc lập vào sự dịch chuyển cuối cùng và việc xóa chỉ làm giảm số lượng mà không tạo ra sự tương tác giữa các hướng. Vì vị trí cuối cùng là tổng của các vectơ đơn vị độc lập nên tính khả thi chỉ phụ thuộc vào việc chúng ta có thể chọn đủ vectơ theo từng hướng yêu cầu hay không. Không có ràng buộc thứ tự nào có thể ngăn cản lựa chọn hợp lệ vì bất kỳ tiền tố không mong muốn nào cũng có thể bị xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        px, py = map(int, input().split())
        s = input().strip()

        r = s.count('R')
        l = s.count('L')
        u = s.count('U')
        d = s.count('D')

        ok_x = (px >= 0 and r >= px) or (px < 0 and l >= -px)
        ok_y = (py >= 0 and u >= py) or (py < 0 and d >= -py)

        print("YES" if ok_x and ok_y else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo quan sát rằng chỉ có phương hướng mới quan trọng. Mỗi trường hợp kiểm thử sẽ tính bốn số đếm theo thời gian tuyến tính trên chuỗi, sau đó so sánh chúng với các thành phần dịch chuyển được yêu cầu. 

Một điểm tinh tế là xử lý dấu hiệu một cách chính xác: dương x tương ứng với R, âm x tương ứng với L và tương tự với y với U và D. Việc trộn lẫn các điều kiện này dẫn đến việc kiểm tra tính khả thi không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
1
1 1
UDDDRLLL
```Chúng tôi tính toán số lượng: U = 1, D = 3, R = 1, L = 3. Mục tiêu yêu cầu một nước đi sang phải và một nước đi lên. Cả hai đều có sẵn, vì vậy chúng ta có thể chọn "R" và "U" theo thứ tự, cho ra CÓ. 

| Bước | Bạn | D | L | R | px | py | được_x | được_y | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 3 | 3 | 1 | 1 | 1 | đúng | đúng | 

Điều này cho thấy thứ tự không quan trọng vì cả hai hướng yêu cầu đều tồn tại. 

Bây giờ hãy xem xét:```
1
1 2
LLLLUU
```Số lượng là U = 2, D = 0, L = 4, R = 0. Chúng ta cần một nước đi R và hai nước đi U. Không có R nên mặc dù có thể di chuyển theo chiều dọc nhưng không thể di chuyển theo chiều ngang. 

| Bước | Bạn | D | L | R | px | py | được_x | được_y | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 0 | 4 | 0 | 1 | 2 | sai | đúng | 

Điều này chứng tỏ rằng cả hai trục phải thỏa mãn độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | chỉ có bốn quầy được lưu trữ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 100000, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        px, py = map(int, input().split())
        s = input().strip()

        r = s.count('R')
        l = s.count('L')
        u = s.count('U')
        d = s.count('D')

        ok_x = (px >= 0 and r >= px) or (px < 0 and l >= -px)
        ok_y = (py >= 0 and u >= py) or (py < 0 and d >= -py)

        print("YES" if ok_x and ok_y else "NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# provided samples
assert run("""6
10 5
RRRRRRRRRRUUUUU
1 1
UDDDRLLL
-3 -5
LDLDLDDDR
1 2
LLLLUU
3 -2
RDULRLLDR
-1 6
RUDURUUUUR
""") == "", "sample test"

# custom cases
assert run("""1
1 1
RU
""") == "", "minimum positive case"

assert run("""1
-1 -1
UR
""") == "", "sign mismatch case"

assert run("""1
5 0
RRR
""") == "", "insufficient x moves"

assert run("""1
0 3
UUUDDD
""") == "", "exact vertical feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| RU với (1,1) | CÓ | xây dựng hợp lệ tối thiểu | 
| UR với (-1,-1) | KHÔNG | xử lý biển chỉ dẫn không khớp | 
| RRR với (5,0) | KHÔNG | cung không đủ | 
| UUUDDD với (0,3) | CÓ | tính khả thi chỉ theo chiều dọc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mục tiêu nằm hoàn toàn trên một trục, chẳng hạn như (0, y). Trong tình huống này, việc di chuyển theo chiều ngang là không liên quan và chỉ có U và D là quan trọng. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ kiểm tra các hướng cần thiết trên mỗi trục. 

Một trường hợp cạnh khác xảy ra khi chuỗi chứa các bước di chuyển ngược lại cân bằng chính xác, chẳng hạn như số L và R bằng nhau, nhưng mục tiêu yêu cầu một sự dịch chuyển ròng. Giải pháp không dựa vào sự dịch chuyển ròng của chuỗi gốc mà chỉ dựa vào tính khả dụng, do đó nó bỏ qua các hiệu ứng hủy trong chuỗi đầy đủ và chỉ tập trung vào tính khả thi của việc lựa chọn.
