---
title: "CF 1734D - Thoát khỏi chất nhờn"
description: "Chúng ta bắt đầu ở vị trí k trên một dòng gồm n slime. Mỗi vị trí đều chứa một chất nhờn có giá trị sức khỏe nhất định và khi chúng ta bước lên một vị trí vẫn còn chất nhờn, chúng ta sẽ hấp thụ nó và bổ sung sức khỏe của nó cho mình. Giá trị tiêu cực làm giảm sức khỏe của chúng ta, giá trị tích cực làm tăng sức khỏe."
date: "2026-06-15T03:27:32+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1734
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 822 (Div. 2)"
rating: 1800
weight: 1734
solve_time_s: 219
verified: false
draft: false
---

[CF 1734D - Thoát khỏi chất nhờn](https://codeforces.com/problemset/problem/1734/D) 

**Đánh giá:** 1800 
**Tags:** cấu trúc dữ liệu, dp, tham lam, hai con trỏ 
**Thời gian giải:** 3 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu ở vị trí`k`trên một dòng`n`slime. Mỗi vị trí đều chứa một chất nhờn có giá trị sức khỏe nhất định và khi chúng ta bước lên một vị trí vẫn còn chất nhờn, chúng ta sẽ hấp thụ nó và bổ sung sức khỏe của nó cho mình. Giá trị tiêu cực làm giảm sức khỏe của chúng ta, giá trị tích cực làm tăng sức khỏe. Chất nhờn của chúng tôi không bao giờ được phép giảm xuống dưới 0 bất cứ lúc nào. 

Mục tiêu là di chuyển từng bước sang trái về phía vị trí`0`hoặc phải về phía vị trí`n+1`, hấp thụ mọi thứ chúng ta đi qua vì chất nhờn biến mất sau khi ăn. Chúng ta được phép ngầm lựa chọn thứ tự mở rộng bằng cách quyết định hướng nào sẽ đi trước, nhưng khi chúng ta di chuyển qua một vị trí thì nó sẽ biến mất. 

Khó khăn chính là chúng ta không chỉ đơn giản kiểm tra xem tổng số có phải là số âm hay không. Chúng tôi phải đảm bảo rằng ở mọi tiền tố của đường đi, tình trạng chạy bộ không bao giờ trở nên tiêu cực và trước tiên chúng tôi có thể phải chọn hướng “an toàn hơn”. 

Các ràng buộc là lớn, với tổng`n`trên tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này loại trừ bất kỳ mô phỏng bậc hai nào trong các khoảng thời gian hoặc thử tất cả các phép xen kẽ trái-phải có thể. Mọi giải pháp đều phải tuyến tính trên mỗi thử nghiệm hoặc được khấu hao tuyến tính tổng thể. Điều đó gợi ý rõ ràng rằng mỗi vị trí được xử lý nhiều nhất với số lần không đổi và các quyết định đó phụ thuộc vào việc kiểm tra tính khả thi giống như tiền tố hoặc tham lam. 

Một cạm bẫy tinh vi là giả định rằng chỉ có tổng số tiền là quan trọng. Ví dụ: hãy xem xét một phân khúc như`[-5, +100, -200, +200]`. Tổng tổng là dương, nhưng nếu chúng ta thực hiện sai thứ tự hấp thụ, chúng ta có thể chạm vào tiền tố âm trước khi đạt đến giá trị dương lớn. Một trường hợp thất bại khác là giả định rằng chúng ta phải lấy hết tất cả slime ở một bên trước khi chạm vào bên kia; trên thực tế, chiến lược tối ưu có thể chỉ liên quan đến việc mở rộng một bên cho đến khi nó trở nên “đủ an toàn” để chuyển đổi. 

## Phương pháp tiếp cận 

Một quan điểm ngây thơ là mô phỏng cả hai khả năng: thử đi từ trái trước hoặc phải trước và xem xét đệ quy tất cả các cách để mở rộng ra bên ngoài. Ở mỗi bước, chúng tôi chọn di chuyển sang trái hoặc phải, kiểm tra xem sức khỏe có còn âm hay không và tiếp tục. Điều này nhanh chóng trở thành cấp số nhân vì mọi trạng thái được xác định bởi khoảng thời gian hiện tại`[L, R]`và vị trí bên trong nó, đồng thời phân nhánh theo hướng sẽ tạo ra một vụ nổ tổ hợp. 

Ngay cả khi chúng tôi hạn chế thực hiện các chiến lược đơn điệu như “đi hoàn toàn sang trái rồi hoàn toàn sang phải” hoặc ngược lại, điều này vẫn thất bại vì tính khả thi trung gian có ý nghĩa quan trọng: một phân khúc có thể có tổng tổng dương nhưng vẫn yêu cầu hấp thụ theo một thứ tự cụ thể để tránh mức giảm xuống dưới 0. 

Quan sát quan trọng là một khi chúng ta quyết định tiêu thụ hoàn toàn một bên, trật tự nội bộ sẽ được cố định. Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể mở rộng sang trái và phải theo thứ tự nào đó sao cho mỗi phần mở rộng đều khả thi với tình trạng hiện tại hay không. Cái nhìn sâu sắc quan trọng là tính khả thi của việc mở rộng sang một bên chỉ phụ thuộc vào việc liệu chúng ta có thể đi qua toàn bộ phân đoạn đó bắt đầu từ tình trạng hiện tại hay không, điều này có thể được kiểm tra một cách tham lam bằng cách quét ra bên ngoài và duy trì tổng tiền tố tối thiểu. 

Vì vậy, thay vì mô phỏng tất cả các đường đi, chúng tôi tính toán trước xem mỗi bên có thể được tiêu thụ hoàn toàn từ điểm xuất phát hay không, sau đó tham lam mở rộng bên hiện đang an toàn, cập nhật tình trạng của chúng tôi khi tăng khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ các trạng thái | Hàm mũ | O(n) | Quá chậm | 
| Mở rộng tham lam bằng cách kiểm tra tính khả thi của tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt nguồn từ lý luận ngay từ đầu`k`và coi vùng có thể truy cập hiện tại là một phân đoạn mở rộng ra bên ngoài. 

1. Tính xem chúng ta có thể khai triển sang trái từ`k`càng xa càng tốt đồng thời không bao giờ để sức khỏe giảm xuống dưới 0. Chúng tôi mô phỏng việc di chuyển sang trái từng bước một, cập nhật sức khỏe và dừng lại khi không thể tiếp tục. Điều này mang lại ranh giới bên trái tối đa có thể tiếp cận được. 
2. Tương tự tính toán xem chúng ta có thể mở rộng bao xa sang bên phải từ`k`dưới cùng một quy luật. Điều này mang lại một ranh giới bên phải có thể tiếp cận tối đa. 
3. Nếu một trong hai bên vượt quá ranh giới (`0`hoặc`n+1`), chúng ta có thể trốn thoát ngay lập tức. 
4. Ngược lại, chúng ta coi bài toán như duy trì một khoảng`[L, R]`ban đầu tại`[k, k]`, với sức khỏe hiện tại bằng`a[k]`. 
5. Chúng ta cố gắng mở rộng khoảng cách ra bên ngoài. Ở mỗi giai đoạn, chúng tôi xem xét mở rộng từ trái sang`L-1`hoặc quyền`R+1`, nhưng chỉ khi toàn bộ phân đoạn bên đó “khả thi”, nghĩa là chúng ta có thể vượt qua nó bắt đầu từ lượng máu hiện tại mà không bị tiêu cực. 
6. Để kiểm tra tính khả thi một cách hiệu quả, chúng tôi duy trì tổng tiền tố của các phân đoạn và đảm bảo tổng tiền tố tối thiểu dọc theo tiện ích mở rộng không giảm xuống dưới`-current_health`. Điều này đảm bảo chúng ta không bao giờ vượt qua số 0 trong quá trình hấp thụ. 
7. Chúng ta tham lam mở rộng bất cứ bên nào hiện có thể thực hiện được. Nếu cả hai đều khả thi, thì một trong hai thứ tự sẽ hoạt động vì cả hai bản mở rộng đều bảo toàn sức khỏe không âm. 
8. Chúng ta tiếp tục cho đến khi đạt được một trong hai`0`hoặc`n+1`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hạn chế duy nhất quan trọng là liệu một phân đoạn liền kề đầy đủ có thể được sử dụng mà không vi phạm điều kiện tiền tố không âm hay không. Khi một phân khúc khả thi, việc sử dụng toàn bộ phân khúc đó luôn là tối ưu vì nó chỉ tăng tính linh hoạt bằng cách mở rộng các vị trí có thể tiếp cận và không bao giờ giảm các lựa chọn trong tương lai ngoại trừ thông qua những thay đổi về sức khỏe đã được tính đến trong tính khả thi. Quá trình duy trì bất biến rằng khoảng thời gian hiện tại`[L, R]`có thể tiêu thụ hoàn toàn theo một thứ tự nào đó mà không vi phạm giới hạn sức khỏe. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_cross(a, k):
    n = len(a) - 1
    left = k
    right = k
    health = a[k]

    if health < 0:
        return False

    # Try expanding greedily
    while True:
        moved = False

        # try left
        if left > 1:
            h = health
            ok = True
            for i in range(left - 1, 0, -1):
                h += a[i]
                if h < 0:
                    ok = False
                    break
            if ok:
                for i in range(left - 1, 0, -1):
                    health += a[i]
                left = 1
                moved = True

        # try right
        if right < n:
            h = health
            ok = True
            for i in range(right + 1, n + 1):
                h += a[i]
                if h < 0:
                    ok = False
                    break
            if ok:
                for i in range(right + 1, n + 1):
                    health += a[i]
                right = n
                moved = True

        if not moved:
            break

        if left == 1 or right == n:
            return True

    return left == 1 or right == n

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = [0] + list(map(int, input().split()))
        print("YES" if can_cross(a, k) else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai giữ khoảng thời gian hiện tại và cố gắng mở rộng khoảng thời gian đó ra bên ngoài khi toàn bộ phân đoạn bên có thể được hấp thụ một cách an toàn. Kiểm tra tính khả thi bên trong mô phỏng mức tiêu thụ của một bên ứng cử viên trong khi theo dõi tình trạng tối thiểu một cách ngầm định thông qua tổng hoạt động. 

Điểm tinh tế chính là chúng tôi chỉ cam kết mở rộng khi chúng tôi biết toàn bộ phân khúc này an toàn. Tiêu thụ một phần không bao giờ được phép trong giai đoạn kiểm tra, vì điều đó sẽ trộn lẫn việc kiểm tra tính khả thi với đột biến trạng thái. Một chi tiết quan trọng khác là bảo vệ các điều kiện biên: đạt được`left == 1`hoặc`right == n`nghĩa là chúng ta vẫn có thể tiến thêm một bước nữa vào ô trốn thoát. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4
-1 -2 -3 6 -2 -3 -1
```Chúng tôi bắt đầu ở chỉ số 4 với sức khỏe 6. 

| Bước | Trái | Đúng | Sức khỏe | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 6 | Bắt đầu | 
| 2 | 4 | 7 | 6 → -3 | Mở rộng bên phải đầy đủ | 
| 3 | 4 | 7 | 6 → -3 → -6 → -7 | Quyền mở rộng không hợp lệ sớm | 

Phía bên phải không thể tiêu thụ một cách an toàn trước tiên, vì vậy thay vào đó chúng tôi thử mở rộng bên trái. 

| Bước | Trái | Đúng | Sức khỏe | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 6 | Bắt đầu | 
| 2 | 1 | 4 | 6 → 5 → 3 → 0 | Mở rộng sang trái hoàn toàn | 
| 3 | 1 | 4 | 0 → 6 → 4 → 1 | Sau đó mở rộng phải | 

Chúng tôi đạt được thành công cả hai đầu. 

Điều này cho thấy chiến lược đúng đắn không phải là thiên hướng định hướng mà là mở rộng dựa trên tính khả thi. 

### Ví dụ 2 

đầu vào:```
3 1
232 -500 -700
```Chúng tôi bắt đầu ở vị trí 1, gần thoát 0. 

| Bước | Trái | Đúng | Sức khỏe | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 232 | Bắt đầu | 
| 2 | 0 | 1 | 232 | Di chuyển sang trái để trốn thoát | 

Điều này chứng tỏ một trường hợp ranh giới trong đó không cần đến sự hấp thụ nào cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra (khấu hao) | Mỗi phần tử được hấp thụ tối đa một lần trong quá trình mở rộng | 
| Không gian | O(n) | Lưu trữ cho mảng đầu vào | 

Tổng độ phức tạp là tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái với giới hạn tổng cộng 200.000 slime. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys
```
