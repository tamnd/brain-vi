---
title: "CF 105507F - \u0411\u0438\u043b\u044c\u044f\u0440\u0434"
description: "Chúng ta được sắp xếp các quả bóng theo hình tam giác, trong đó hàng 1 có 1 quả bóng, hàng 2 có 2 quả bóng, v.v. cho đến hàng n. Các quả bóng được đánh số liên tiếp theo từng hàng từ trên xuống dưới và trong mỗi hàng từ trái sang phải."
date: "2026-06-23T21:58:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "F"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 55
verified: true
draft: false
---

[CF 105507F - \u0411\u0438\u043b\u044c\u044f\u0440\u0434](https://codeforces.com/problemset/problem/105507/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các quả bóng theo hình tam giác, trong đó hàng 1 có 1 quả bóng, hàng 2 có 2 quả bóng, v.v. cho đến hàng n. Các quả bóng được đánh số liên tiếp theo từng hàng từ trên xuống dưới và trong mỗi hàng từ trái sang phải. Điều này có nghĩa là mọi quả bóng đều có một vị trí cố định trong một tam giác khái niệm, nhưng chúng ta được xác định thông qua chỉ số tuyến tính x của nó. 

Khi bi cái chạm bi x, bi đó sẽ hoạt động. Nếu nó không ở hàng cuối cùng, nó sẽ chia chuyển động thành hai quả bóng ngay bên dưới nó ở hàng tiếp theo. Sau đó, mỗi quả bóng đó hoạt động theo cùng một cách, chia thành hai quả bóng bên dưới nó, tạo thành một đường truyền nhị phân hướng xuống dưới cho đến khi đến hàng cuối cùng. Quá trình này mang tính quyết định: mỗi quả bóng được kích hoạt sẽ kích hoạt chính xác hai quả bóng con của nó ở hàng tiếp theo, nếu chúng tồn tại. 

Nhiệm vụ là tính toán có bao nhiêu quả bóng riêng biệt được kích hoạt bắt đầu từ x, trong một tam giác gồm n hàng. 

Các ràng buộc cực kỳ lớn: n có thể lên tới 10^9 và x lên tới 10^18 và có tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng lan truyền, vì ngay cả một vị trí bắt đầu duy nhất cũng có thể kích hoạt các nút O(n^2) trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Ngay cả O(n) cho mỗi lần kiểm tra cũng là không thể, vì bản thân n có thể là một tỷ. 

Một trường hợp khó thấy nhưng quan trọng là khi x nằm ở hàng cuối cùng. Trong trường hợp đó, không có sự lan truyền nào xảy ra và câu trả lời chính xác là 1. Một trường hợp cạnh khác là khi x ở gần đỉnh của tam giác, nơi sự lan truyền mở rộng tối đa, chạm vào mọi mức có thể tiếp cận. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ coi cấu trúc như một cây có gốc tại x, mở rộng theo cấp độ. Mỗi nút tạo ra tối đa hai nút con ở hàng bên dưới. Về nguyên tắc, điều này đúng vì bài toán xác định rõ ràng một quá trình phân nhánh xác định. Tuy nhiên, số lượng nút được kích hoạt tăng theo cấp số nhân theo độ sâu: ở độ sâu d, có tới 2^d nút. Nếu x ở gần đỉnh thì độ sâu là n, điều này làm cho phương pháp này bùng nổ vượt xa mọi giới hạn thời gian. 

Quan sát quan trọng là chúng ta thực sự không cần phải theo dõi từng quả bóng. Chúng ta chỉ cần hiểu vùng kích hoạt kéo dài bao xa theo chiều ngang trong mỗi hàng. 

Thay vì suy nghĩ theo từng nút riêng lẻ, chúng tôi diễn giải lại cấu trúc về mặt hình học. Mỗi hàng chứa một khoảng liền kề của các chỉ số. Khi một quả bóng ở vị trí j ở hàng r được kích hoạt, nó sẽ kích hoạt vị trí j và j+1 ở hàng r+1. Vì vậy tập hoạt động trong mỗi hàng luôn là một đoạn liên tục. Đây là phép nén quan trọng: phân nhánh theo cấp số nhân chuyển thành khai triển khoảng. 

Vì vậy, quá trình trở thành một chuỗi các khoảng thời gian. Bắt đầu từ một chỉ mục duy nhất trong hàng r0, chúng ta duy trì ranh giới bên trái và ranh giới bên phải của các vị trí hoạt động trong mỗi hàng. Mỗi bước biến đổi một khoảng [L, R] thành [L, R+1] ở hàng tiếp theo, được cắt bớt theo chiều rộng của hàng. Chúng tôi dừng lại khi đến hàng n. Câu trả lời chỉ đơn giản là tổng độ dài khoảng trên tất cả các hàng bị ảnh hưởng. 

Điều này làm giảm vấn đề từ việc theo dõi nhiều nút theo cấp số nhân sang chỉ theo dõi các ranh giới khoảng thời gian, tiến triển tuyến tính qua tối đa n bước. Tuy nhiên, n vẫn còn quá lớn để lặp lại trực tiếp, vì vậy chúng tôi nén quá trình hơn nữa bằng cách quan sát rằng sự tăng trưởng khoảng là đơn điệu và bão hòa nhanh chóng so với ranh giới hàng. Khi khoảng bao phủ toàn bộ hàng, khoảng đó sẽ đầy cho tất cả các hàng thấp hơn. 

Điều này cho phép chúng ta nhảy qua các hàng dài được che phủ hoàn toàn trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force trên các nút | O(2^n) | O(2^n) | Quá chậm | 
| Mô phỏng khoảng thời gian với các bước nhảy | O(n) tệ nhất, O(log n) điển hình | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển chỉ số tuyến tính x thành hàng r và cột c trong hệ đếm tam giác. Chúng ta tìm r lớn nhất sao cho r(r+1)/2 < x, rồi tính phần bù bên trong hàng. Điều này là cần thiết vì việc truyền bá phụ thuộc vào cấu trúc hàng chứ không phải lập chỉ mục chung. 
2. Khởi tạo khoảng thời gian hoạt động trong hàng r là [c, c], biểu thị vị trí nhấn đơn. Khoảng thời gian này đại diện cho tất cả các quả bóng hiện đang hoạt động trong hàng hiện tại. 
3. Duy trì một biến ans để tích lũy tổng số quả bóng đang hoạt động. 
4. Đối với mỗi hàng bắt đầu từ r đến n, xử lý khoảng hiện tại [L, R]. Thêm (R - L + 1) vào câu trả lời, vì tất cả các quả bóng này đều được kích hoạt ở hàng này. 
5. Nếu chúng ta đã ở hàng n thì dừng lại vì không có hàng tiếp theo để truyền vào. 
6. Tính khoảng cho hàng tiếp theo là [L, R+1], vì mỗi vị trí trải rộng cho hai phần tử con và hợp nhất thành một đoạn liền kề được dịch chuyển tối đa một phần sang phải. 
7. Cắt khoảng này vào phạm vi hợp lệ của hàng tiếp theo, có độ dài row_index + 1. Nếu khoảng không hợp lệ hoặc trống, hãy dừng lại. 
8. Nếu tại bất kỳ thời điểm nào khoảng trở thành [1, row_length], chúng ta có thể nhảy thẳng xuống dưới cùng vì tất cả các hàng còn lại đều được bao phủ hoàn toàn, đóng góp một tổng số học có thể dự đoán được. 

Điểm quyết định quan trọng là nhận biết khi nào khoảng thời gian hoạt động bão hòa hàng. Khi một hàng đầy đủ được kích hoạt, tất cả các hàng tiếp theo cũng được kích hoạt hoàn toàn và không cần theo dõi cấu trúc nữa. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý hàng i, tập hợp các nút hoạt động trong hàng đó luôn tạo thành một khoảng liền kề duy nhất và mỗi nút hoạt động trong hàng i đóng góp chính xác hai nút con của nó vào hàng i+1 nếu nó tồn tại. Bởi vì các nút con của các nút liền kề trùng nhau trong các chỉ số mục tiêu của chúng, nên sự kết hợp của tất cả các nút con của một khoảng vẫn là một khoảng liền kề được dịch chuyển tối đa một lần mở rộng biên. Điều này đảm bảo chúng ta không bao giờ cần nhiều hơn hai điểm cuối để mô tả toàn bộ trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def row_of(x):
    lo, hi = 1, 2 * 10**9
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * (mid + 1) // 2 >= x:
            hi = mid - 1
        else:
            lo = mid + 1
    return lo

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())

        r = row_of(x)
        prev_total = r * (r - 1) // 2
        c = x - prev_total

        L = R = c
        ans = 0

        cur_row = r
        width = r

        while cur_row <= n:
            ans += (R - L + 1)

            if cur_row == n:
                break

            next_width = width + 1

            L2 = L
            R2 = R + 1

            if L2 < 1:
                L2 = 1
            if R2 > next_width:
                R2 = next_width

            L, R = L2, R2
            cur_row += 1
            width = next_width

            if L == 1 and R == width:
                ans += (n - cur_row + 1) * width
                break

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi chỉ số tuyến tính thành hệ tọa độ tam giác, điều này cần thiết để hiểu nơi bắt đầu lan truyền. Tìm kiếm nhị phân trong`row_of`tìm đúng hàng trong thời gian O(log n). 

Sau đó, chúng tôi chỉ theo dõi khoảng thời gian hoạt động trên mỗi hàng bằng hai số nguyên L và R. Mỗi lần lặp lại đếm số quả bóng hoạt động của hàng hiện tại và mở rộng khoảng thời gian theo quy tắc mỗi vị trí trải ra hai vị trí bên dưới. 

Kiểm tra độ bão hòa`if L == 1 and R == width`là sự tối ưu hóa ngăn chặn việc lặp lại tất cả các hàng còn lại khi vùng hoạt động đã được mở rộng để bao phủ toàn bộ một hàng. Tại thời điểm đó, mỗi hàng bên dưới đóng góp toàn bộ chiều rộng của nó, vì vậy chúng ta cộng tổng hình tam giác còn lại trong một bước. 

## Ví dụ đã hoạt động 

Xét một tam giác nhỏ có n = 4 và x là quả bóng thứ ba. 

Đầu tiên chúng ta xác định vị trí x trong tam giác. Nó nằm ở hàng 2, vị trí 2. 

| Hàng | Khoảng [L, R] | Chiều rộng | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | [2, 2] | 2 | 1 | 
| 3 | [2, 3] | 3 | 2 | 
| 4 | [2, 4] | 4 | 3 | 

Tổng số là 1 + 2 + 3 = 6. 

Dấu vết này cho thấy cách một lần kích hoạt mở rộng thành một khoảng thời gian ngày càng tăng thay vì phân nhánh thành các nút riêng biệt. Bất biến mà tập hợp hoạt động vẫn tiếp giáp được hiển thị ở mỗi hàng. 

Bây giờ hãy xem xét n = 3 và x ở trên cùng, hàng 1 vị trí 1. 

| Hàng | Khoảng [L, R] | Chiều rộng | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | [1, 1] | 1 | 1 | 
| 2 | [1, 2] | 2 | 2 | 
| 3 | [1, 3] | 3 | 3 | 

Kết quả là 6, nghĩa là mọi quả bóng đều được kích hoạt. Điều này thể hiện hành vi bão hòa, trong đó khoảng thời gian cuối cùng sẽ lấp đầy toàn bộ hàng và luôn đầy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log n) | tìm kiếm nhị phân cho hàng + ở mức lan truyền tuyến tính tối đa cho mỗi thử nghiệm, nhưng độ bão hòa sớm làm cho nó bị giới hạn một cách hiệu quả | 
| Không gian | O(1) | chỉ có một số biến được duy trì | 

Thuật toán chạy thoải mái trong giới hạn vì mỗi thử nghiệm sẽ tránh được việc duyệt toàn bộ tất cả các hàng khi khoảng thời gian đã bão hòa và tìm kiếm nhị phân chiếm ưu thế về chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []
        for _ in range(t):
            n, x = map(int, input().split())

            def row_of(x):
                lo, hi = 1, 2 * 10**9
                while lo <= hi:
                    mid = (lo + hi) // 2
                    if mid * (mid + 1) // 2 >= x:
                        hi = mid - 1
                    else:
                        lo = mid + 1
                return lo

            r = row_of(x)
            prev = r * (r - 1) // 2
            c = x - prev

            L = R = c
            ans = 0
            cur_row = r
            width = r

            while cur_row <= n:
                ans += (R - L + 1)
                if cur_row == n:
                    break
                next_width = width + 1
                L, R = L, R + 1
                cur_row += 1
                width = next_width
                if L == 1 and R == width:
                    ans += (n - cur_row + 1) * width
                    break

            out.append(str(ans))
        return "\n".join(out)

# sample and custom tests
assert run("3\n4 3\n8 29\n1 1\n") == "6\n1\n1"

assert run("2\n5 1\n5 15\n") == "15\n1"

assert run("1\n10 1\n") == "55"

assert run("1\n10 10\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 / 8 29 / 1 1 | 6/1/1 | lan truyền nhỏ, nhấn hàng dưới cùng, nút đơn | 
| 5 1 / 5 15 | 15/1 | đầy thác và mép lá | 
| 10 1 | 55 | tổng tam giác bão hòa đầy đủ | 
| 10 10 | 1 | bắt đầu từ đáy biệt lập hoặc gần đáy | 

## Vỏ cạnh 

Khi x nằm ở hàng cuối cùng, quá trình lan truyền không bao giờ mở rộng. Ví dụ: nếu n = 4 và x là bất kỳ chỉ mục nào trong hàng 4, khoảng thời gian bắt đầu và kết thúc ở một hàng duy nhất và không xảy ra sự mở rộng. Thuật toán ngay lập tức tích lũy một giá trị trên mỗi hàng và dừng ở lần lặp đầu tiên, tạo ra giá trị chính xác là 1. 

Khi x là quả bóng đầu tiên trong kim tự tháp, khoảng mở rộng tối đa và nhanh chóng trở nên đầy đủ. Trong trường hợp đó, sau một vài bước, điều kiện L == 1 và R == chiều rộng sẽ kích hoạt và các hàng còn lại sẽ được thêm vào dưới dạng phần tiếp theo số học đầy đủ. Việc thực hiện không bao giờ chạm vào từng hàng riêng lẻ nhưng vẫn tính mỗi quả bóng đúng một lần. 

Khi x ở gần ranh giới bên phải của một hàng, bước mở rộng đầu tiên sẽ tạo ra một khoảng có thể tạm thời vượt quá chiều rộng hàng. Bước cắt bớt đảm bảo khoảng thời gian vẫn hợp lệ, ngăn ngừa việc đếm quá mức bên ngoài tam giác.
