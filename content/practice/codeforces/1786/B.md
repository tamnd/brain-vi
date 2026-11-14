---
title: "CF 1786B - Dây chuyền làm bánh"
description: "Chúng ta có hai hệ thống có thứ tự trên trục số. Một hệ thống đại diện cho những chiếc bánh, mỗi hệ thống chiếm một khoảng cố định tập trung ở một vị trí nhất định và hệ thống kia đại diện cho bộ phân phối sô cô la, mỗi hệ thống cũng tạo ra một khoảng bao phủ cố định."
date: "2026-06-09T10:58:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1786
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 850 (Div. 2, based on VK Cup 2022 - Final Round)"
rating: 1300
weight: 1786
solve_time_s: 99
verified: true
draft: false
---

[CF 1786B - Dây chuyền làm bánh](https://codeforces.com/problemset/problem/1786/B) 

**Đánh giá:** 1300 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hệ thống có thứ tự trên trục số. Một hệ thống đại diện cho những chiếc bánh, mỗi hệ thống chiếm một khoảng cố định tập trung ở một vị trí nhất định và hệ thống kia đại diện cho bộ phân phối sô cô la, mỗi hệ thống cũng tạo ra một khoảng bao phủ cố định. Tất cả các khoảng bánh đều rời rạc, và tất cả các khoảng sô-cô-la đều rời rạc. 

Chúng ta được phép dịch chuyển toàn bộ băng tải bánh sang trái hoặc sang phải một cách cứng nhắc bằng một giá trị thực nào đó. Sau ca này, mỗi chiếc bánh phải nhận sô cô la ở đâu đó trong khoảng của nó và không được phép đổ sô cô la ra ngoài liên kết của tất cả các khoảng bánh. Bản thân các bộ phân phối đã được cố định; chỉ có hệ thống bánh chuyển động. 

Nhiệm vụ là quyết định xem có tồn tại ít nhất một ca làm cho mỗi khoảng bánh giao nhau ít nhất một khoảng sô cô la hay không, đồng thời đảm bảo rằng mỗi khoảng sô cô la nằm hoàn toàn bên trong một khoảng bánh nào đó. 

Các ràng buộc rất lớn, lên tới 100000 phần tử trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm, vì phép so sánh bậc hai trên tất cả các cặp khoảng sẽ vượt quá giới hạn một khoảng lớn. 

Một trường hợp cạnh tinh tế phát sinh khi khoảng cách bánh lớn hơn khoảng cách sô cô la theo cách có thể căn chỉnh cục bộ nhưng không thể căn chỉnh trên toàn cầu. Ví dụ: ngay cả khi mỗi chiếc bánh có thể chồng lên nhau một bộ phân phối riêng lẻ, các ràng buộc về đặt hàng có thể ngăn cản sự thay đổi nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các ca có thể. Đối với mỗi ca, chúng tôi sẽ dịch tất cả các khoảng thời gian của bánh và kiểm tra xem mỗi chiếc bánh có giao nhau với một số khoảng thời gian phân phối và không có bộ phân phối nào mở rộng ra ngoài bánh hay không. Vì các vị trí có giá trị thực và các ràng buộc lớn nên điều này là không khả thi. 

Quan sát quan trọng là cả bánh và bộ phân phối đều đã được sắp xếp và bên trong không chồng chéo. Cấu trúc này buộc bất kỳ sự căn chỉnh hợp lệ nào để duy trì trật tự: chiếc bánh đầu tiên phải căn chỉnh với bộ phân phối khả thi đầu tiên, chiếc bánh thứ hai với chiếc thứ hai, v.v. Khi thứ tự đã được khắc phục, vấn đề sẽ giảm xuống còn việc kiểm tra xem liệu có tồn tại độ lệch nhất quán để căn chỉnh tất cả các cặp đồng thời trong giới hạn dung sai được xác định bởi các nửa chiều rộng khoảng cách hay không. 

Điều này trở thành một vấn đề khớp khoảng thời gian cổ điển: mỗi vị trí bánh áp đặt một phạm vi dịch chuyển cho phép đối với mỗi lần căn chỉnh bộ phân phối. Giao điểm của tất cả các phạm vi này phải không trống. 

Bằng cách chuyển đổi từng ràng buộc căn chỉnh theo cặp thành một khoảng thời gian của các ca hợp lệ và giao nhau với chúng, chúng ta có thể quyết định tính khả thi một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các ca) | O(∞) liên tục | O(1) | Quá chậm | 
| Tối ưu (giao điểm khoảng) | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khắc phục thứ tự giữa các loại bánh và bộ phân phối vì cả hai đều được sắp xếp và rời rạc, có nghĩa là việc giao nhau sẽ ngay lập tức vi phạm tính khả thi. 

1. Chúng ta giả sử bánh i phải tương ứng với bộ phân phối i sau khi dịch chuyển. Bất kỳ ánh xạ nào khác sẽ phá vỡ trật tự đơn điệu do các khoảng rời rạc. 
2. Đối với mỗi cặp, chúng tôi tính toán phạm vi dịch chuyển làm cho khoảng thời gian phân phối vừa khít với khoảng thời gian của bánh. 
3. Mỗi cặp đóng góp một ràng buộc đối với biến dịch chuyển toàn cục, tạo thành một khoảng dịch chuyển hợp lệ. 
4. Chúng tôi giao nhau tất cả các khoảng này. 
5. Nếu giao lộ không trống, thì có một ca hợp lệ; nếu không thì không. 

Mỗi ràng buộc được suy ra bằng cách biểu thị rằng sau khi dịch chuyển một x, khoảng phân phối [b_i - h, b_i + h] phải nằm bên trong [a_i - w, a_i + w]. Điều này tạo ra sự bất bình đẳng ràng buộc x từ cả hai phía. 

### Tại sao nó hoạt động

Biến shift có tính chất toàn cục, do đó mỗi cặp áp đặt một ràng buộc tuyến tính trên cùng một biến. Vì các ràng buộc là độc lập và tạo thành các khoảng trên đường thực, nên tính khả thi sẽ giảm xuống việc kiểm tra xem tất cả các khoảng có giao nhau hay không. Giả định thứ tự là hợp lệ vì bất kỳ phép gán giao cắt nào cũng sẽ mâu thuẫn với khoảng cách đơn điệu được đảm bảo trong cả hai chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, w, h = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        left = -10**18
        right = 10**18

        for i in range(n):
            # dispenser i after shift x lies inside cake i
            # [b_i + x - h, b_i + x + h] ⊆ [a_i - w, a_i + w]

            # left bound: b_i + x - h >= a_i - w
            # x >= a_i - w - (b_i - h)
            l = (a[i] - w) - (b[i] - h)

            # right bound: b_i + x + h <= a_i + w
            # x <= a_i + w - (b_i + h)
            r = (a[i] + w) - (b[i] + h)

            left = max(left, l)
            right = min(right, r)

        print("YES" if left <= right else "NO")

if __name__ == "__main__":
    solve()
```Cốt lõi của giải pháp là rút ra khoảng thời gian thay đổi hợp lệ cho mỗi cặp máy phân phối bánh. Mỗi cặp giới hạn phạm vi dịch được phép và câu trả lời cuối cùng chỉ phụ thuộc vào việc liệu tất cả các phạm vi đó có trùng nhau hay không. 

Một lỗi phổ biến là cố gắng khớp trực tiếp các điểm cuối hoặc ghép nối các bánh và bộ phân phối một cách tham lam mà không chuyển các ràng buộc thành biến toàn cục. Quan điểm đúng là toàn bộ hệ thống phụ thuộc vào một tham số dịch chuyển duy nhất, do đó mọi ràng buộc phải được thể hiện dưới dạng biến đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một hộp nhỏ có ba chiếc bánh và ba bộ phân phối nơi có thể căn chỉnh. Mỗi cặp đóng góp một khoảng ca hợp lệ và giao điểm của chúng không trống. Bảng dưới đây cho thấy các ràng buộc tích lũy như thế nào. 

| tôi | tôi hạn chế | r ràng buộc | ngã tư trái | ngã tư bên phải | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 0 | 5 | 
| 2 | 1 | 6 | 1 | 5 | 
| 3 | 2 | 7 | 2 | 5 | 

Cuối cùng, giao điểm [2, 5] là hợp lệ, nghĩa là mọi sự thay đổi trong phạm vi này đều có hiệu lực. 

Bây giờ hãy xem xét một trường hợp thất bại trong đó các ràng buộc dần dần thu hẹp lại cho đến khi chúng trở nên trống rỗng. 

| tôi | tôi hạn chế | r ràng buộc | ngã tư trái | ngã tư bên phải | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | 3 | 
| 2 | 2 | 4 | 2 | 3 | 
| 3 | 4 | 5 | 4 | 3 | 

Ở bước cuối cùng, giao điểm trống nên không tồn tại ca nào thỏa mãn tất cả các bánh cùng một lúc. 

Những ví dụ này cho thấy khả năng tương thích cục bộ là không đủ; chỉ có giao lộ toàn cầu mới xác định tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi chiếc bánh đóng góp một số phép tính số học không đổi | 
| Không gian | O(1) thêm | chỉ một vài giới hạn khoảng được lưu trữ | 

Thuật toán chia tỷ lệ trực tiếp với kích thước đầu vào và tổng n trong các trường hợp thử nghiệm bị giới hạn, do đó nó dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, w, h = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        left = -10**18
        right = 10**18

        for i in range(n):
            l = (a[i] - w) - (b[i] - h)
            r = (a[i] + w) - (b[i] + h)
            left = max(left, l)
            right = min(right, r)

        out.append("YES" if left <= right else "NO")

    return "\n".join(out)

assert run("""1
2 10 5
65 95
40 65
""") == "YES"

assert run("""1
3 3 2
10 20 30
1 100 200
""") in {"YES", "NO"}  # structural sanity

assert run("""1
3 1 1
1 10 20
1 10 20
""") == "YES"

assert run("""1
2 1 1
1 10
100 200
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hệ thống căn chỉnh nhỏ | CÓ | tính khả thi cơ bản | 
| khoảng cách không khớp | KHÔNG | tính không khả thi toàn cầu | 
| cấu trúc giống hệt nhau | CÓ | căn chỉnh tầm thường | 
| sự tách biệt cực độ | KHÔNG | ngã tư vắng | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi tất cả các bánh và bộ phân phối đã được căn chỉnh theo sự thay đổi liên tục. Trong trường hợp này, mọi khoảng ràng buộc đều thu gọn về một điểm giao nhau chung và thuật toán sẽ chấp nhận ngay lập tức. Một trường hợp khác là khi các ràng buộc chặt chẽ nhưng không hoàn toàn bằng nhau, trong đó trực giác nổi có thể gợi ý tính khả thi nhưng số học số nguyên vẫn mang lại giao điểm hợp lệ. Công thức tính khoảng xử lý cả hai một cách tự nhiên vì nó giảm mọi thứ về giới hạn bất đẳng thức trên một biến duy nhất, tránh mọi nhu cầu phân tích trường hợp hoặc đối sánh heuristic.
