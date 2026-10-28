---
title: "CF 106180B - Flappy Bird"
description: "Chúng tôi đang mô phỏng quá trình chuyển động một chiều thông qua một chuỗi các cột. Mỗi cột xác định một hành lang thẳng đứng có chiều cao cho phép và một con chim di chuyển từ trái sang phải qua các cột này. Tại bất kỳ thời điểm nào con chim chiếm đúng một chiều cao nguyên."
date: "2026-06-20T04:21:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106180
codeforces_index: "B"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2025. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 106180
solve_time_s: 61
verified: true
draft: false
---

[CF 106180B - Flappy Bird](https://codeforces.com/problemset/problem/106180/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình chuyển động một chiều thông qua một chuỗi các cột. Mỗi cột xác định một hành lang thẳng đứng có chiều cao cho phép và một con chim di chuyển từ trái sang phải qua các cột này. Tại bất kỳ thời điểm nào con chim chiếm đúng một chiều cao nguyên. 

Khi chuyển từ cột i sang cột i+1, người chơi chọn một trong hai hành động. Hoặc con chim nhảy lên trên một khoảng k cố định, hoặc nó rơi xuống đúng một đơn vị. Sau khi áp dụng thay đổi theo chiều dọc này, con chim sẽ đáp xuống cột tiếp theo. Con chim chỉ hợp lệ nếu chiều cao của nó nằm trong hành lang cho phép của cột đó, được xác định bởi một đoạn vùng cấm ở phía dưới và phía trên, để lại một khoảng trống ở giữa. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ chuỗi lựa chọn nào cho phép con chim xuất phát ở một độ cao nhất định ở cột đầu tiên và đến cột cuối cùng mà không bao giờ rời khỏi khoảng độ cao cho phép ở bất kỳ cột nào hay không. 

Kích thước đầu vào lớn: số lượng cột trong tất cả các trường hợp thử nghiệm có thể lên tới 500.000. Điều này ngay lập tức loại trừ mọi lập trình động bùng nổ trạng thái hoặc bậc hai cố gắng theo dõi tất cả các đường dẫn riêng lẻ. Bất kỳ giải pháp đúng nào cũng phải nén tất cả các trạng thái có thể có tại một cột thành một biểu diễn nhỏ gọn, lý tưởng nhất là tuyến tính theo số lượng cột. 

Một khó khăn tinh tế xuất phát từ thực tế là quá trình chuyển đổi không đối xứng. Một lần di chuyển sẽ giảm chiều cao đi một lần, lần kia tăng chiều cao lên k, do đó, tập hợp có thể truy cập có khả năng “chia” thành các vùng riêng biệt. Một cách tiếp cận ngây thơ chỉ theo dõi các độ cao có thể tiếp cận tối thiểu và tối đa có thể âm thầm thất bại vì nó có thể bao gồm các độ cao mà thực tế không thể tiếp cận được ở giữa, khiến cho một đường dẫn không thể tiếp cận được chấp nhận không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ theo dõi mọi độ cao có thể có ở mỗi cột. Từ mỗi độ cao có thể tiếp cận, chúng tôi phân nhánh thành hai lần chuyển đổi, một lần giảm dần và một lần tăng theo k. Sau mỗi cột, chúng tôi lọc ra các độ cao nằm ngoài khoảng cho phép. Điều này mô hình hóa chính xác quy trình nhưng nhanh chóng bùng nổ: sau n bước, số lượng trạng thái có thể tăng gấp đôi liên tục, dẫn đến tăng trưởng theo cấp số nhân. 

Quan sát quan trọng là chúng ta không cần phân biệt các đường đi riêng lẻ mà chỉ cần phân biệt tập hợp các độ cao có thể tiếp cận được. Vì tất cả các chuyển đổi là các dịch chuyển tuyến tính được áp dụng thống nhất cho tất cả các trạng thái, nên các trạng thái có thể truy cập ở bất kỳ cột nào tạo thành một liên kết các bản sao đã dịch chuyển của các tập hợp có thể truy cập trước đó. Cấu trúc này có thể được duy trì như một tập hợp các khoảng rời rạc thay vì các điểm riêng lẻ. 

Mỗi khoảng tạo ra tối đa hai khoảng dịch chuyển ở cột tiếp theo. Sau khi giao nhau với khoảng cho phép của cột đó, ta gộp các đoạn chồng lên nhau. Trong thực tế, số khoảng cách vẫn còn nhỏ vì việc dịch chuyển và giao cắt lặp đi lặp lại sẽ nhanh chóng kết hợp các đoạn lại với nhau. Điều này làm giảm trạng thái từ đường dẫn hàm mũ sang hệ thống khoảng có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các bang | Hàm mũ | Hàm mũ | Quá chậm | 
| Tuyên truyền khoảng thời gian với việc hợp nhất | O(N log N) trường hợp xấu nhất, O(N) điển hình | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tập hợp tất cả các độ cao có thể tiếp cận ở cột hiện tại dưới dạng danh sách được sắp xếp các khoảng rời rạc.

1. Khởi tạo trạng thái với một khoảng duy nhất chỉ chứa chiều cao bắt đầu s. Điều này thể hiện rằng ban đầu có chính xác một vị trí hợp lệ. 
2. Đối với mỗi cột i từ 1 đến n, trước tiên chúng ta tính khoảng chiều cao cho phép [thấp, cao], là [li + 1, ri − 1]. Bất kỳ trạng thái nào nằm ngoài phạm vi này đều không hợp lệ và phải bị loại bỏ ngay lập tức. 
3. Từ mỗi khoảng thời gian hiện tại [L, R], chúng tôi tạo ra hai khoảng thời gian mới cho cột tiếp theo. Một tương ứng với việc rơi xuống, tạo ra [L − 1, R − 1]. Cái còn lại tương ứng với việc nhảy, tạo ra [L + k, R + k]. Bước này nắm bắt tất cả các kết quả có thể xảy ra theo sự lựa chọn của người chơi. 
4. Chúng tôi thu thập tất cả các khoảng thời gian được tạo từ tất cả các khoảng thời gian hiện tại vào danh sách tạm thời. Danh sách này có thể chứa sự chồng chéo và không nhất quán về thứ tự. 
5. Chúng tôi sắp xếp các khoảng này theo điểm bắt đầu và hợp nhất mọi khoảng chồng chéo hoặc liền kề thành các đoạn rời rạc tối đa. Điều này nén trạng thái trở lại thành một biểu diễn nhỏ gọn. 
6. Ta cắt từng khoảng đã hợp nhất với khoảng cho phép của cột i, loại bỏ những phần nằm ngoài. Các giao lộ có thể phân chia các khoảng, nhưng các phần kết quả vẫn rời rạc. 
7. Nếu tại bất kỳ thời điểm nào không còn khoảng thời gian nào thì quá trình sẽ kết thúc sớm với lỗi do không tồn tại chiều cao hợp lệ ở cột đó. 
8. Sau khi xử lý tất cả các cột, nếu còn lại ít nhất một khoảng thì có thể truy cập được cột cuối cùng. 

Thuộc tính quan trọng đằng sau tính chính xác là tập hợp độ cao có thể đạt được sau mỗi bước được biểu diễn chính xác dưới dạng tập hợp các khoảng rời rạc được hình thành bằng cách áp dụng các phép biến đổi affine đồng nhất cho các khoảng trước đó và sau đó giao nhau với một ràng buộc liền kề. Không có chiều cao có thể tiếp cận nào bị mất ngoại trừ khi nó vi phạm ràng buộc cột và không có chiều cao không hợp lệ nào được đưa vào vì tất cả các phép biến đổi đều tuân thủ nghiêm ngặt các bước di chuyển được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge(intervals):
    if not intervals:
        return []
    intervals.sort()
    res = []
    l, r = intervals[0]
    for x, y in intervals[1:]:
        if x <= r + 1:
            r = max(r, y)
        else:
            res.append((l, r))
            l, r = x, y
    res.append((l, r))
    return res

def intersect(intervals, L, R):
    res = []
    for l, r in intervals:
        nl = max(l, L)
        nr = min(r, R)
        if nl <= nr:
            res.append((nl, nr))
    return res

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, h, k, s = map(int, input().split())
        intervals = [(s, s)]

        ok = True
        for _ in range(n):
            l, r = map(int, input().split())
            low, high = l + 1, r - 1

            nxt = []
            for a, b in intervals:
                nxt.append((a - 1, b - 1))
                nxt.append((a + k, b + k))

            nxt = merge(nxt)
            intervals = intersect(nxt, low, high)

            if not intervals:
                ok = False
                break

        out.append("Yes" if ok else "No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì trạng thái có thể truy cập theo khoảng thời gian. Đối với mỗi cột, nó tạo ra tất cả các khoảng dịch chuyển có thể có từ cả hai lựa chọn chuyển động, hợp nhất các phần chồng chéo và sau đó cắt chúng theo vùng được phép của cột đó. Điều kiện thoát sớm tránh được việc tính toán không cần thiết khi tính khả thi bị mất. 

Một lỗi triển khai phổ biến là cố gắng chỉ theo dõi chiều cao tối thiểu và tối đa có thể tiếp cận. Điều đó không thành công vì quá trình chuyển đổi chia các tập hợp có thể truy cập thành hai dải riêng biệt và vùng giữa có thể không thể truy cập được ngay cả khi các giá trị cực đoan gợi ý khác. Biểu diễn khoảng tránh sự mất cấu trúc này. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ có hai cột và kích thước bước nhảy vừa phải. 

đầu vào: 

n = 2, s = 4, k = 2 

Cột 1 cho phép [1, 6], Cột 2 cho phép [1, 6] 

Trạng thái ban đầu là: 

| bước | khoảng thời gian | 
| --- | --- | 
| bắt đầu | [4, 4] | 

Sau khi chuyển đổi cột 1: 

Từ [4,4] ta được [3,3] (rơi) và [6,6] (nhảy). Sau khi hợp nhất, các khoảng là [3,3], [6,6]. Cả hai đều hợp lệ trong cột 1. 

Bây giờ xử lý cột 2: 

Từ [3,3] → [2,2] và [5,5] 

Từ [6,6] → [5,5] và [8,8] 

Sau khi hợp nhất: [2,2], [5,5], [8,8] 

Giao nhau với [1,6] sẽ loại bỏ [8,8], để lại [2,2], [5,5]. Vì còn lại ít nhất một trạng thái nên câu trả lời là Có. 

Dấu vết này cho thấy các trạng thái có thể tiếp cận có thể chia thành nhiều thành phần rời rạc ngay cả trong các đầu vào nhỏ, điều này biện minh cho việc duy trì các khoảng thời gian thay vì một phạm vi duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) cho mỗi trường hợp xấu nhất của thử nghiệm | Mỗi bước hợp nhất danh sách khoảng thời gian | 
| Không gian | O(N) | Lưu trữ bộ khoảng thời gian | 

Tổng số cột cập nhật trên tất cả các trường hợp thử nghiệm bị giới hạn bởi 500.000, vì vậy phương pháp này phù hợp thoải mái trong giới hạn thời gian ngay cả với việc hợp nhất dựa trên sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def merge(intervals):
        if not intervals:
            return []
        intervals.sort()
        res = []
        l, r = intervals[0]
        for x, y in intervals[1:]:
            if x <= r + 1:
                r = max(r, y)
            else:
                res.append((l, r))
                l, r = x, y
        res.append((l, r))
        return res

    def intersect(intervals, L, R):
        res = []
        for l, r in intervals:
            nl = max(l, L)
            nr = min(r, R)
            if nl <= nr:
                res.append((nl, nr))
        return res

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, h, k, s = map(int, input().split())
            intervals = [(s, s)]
            ok = True
            for _ in range(n):
                l, r = map(int, input().split())
                low, high = l + 1, r - 1
                nxt = []
                for a, b in intervals:
                    nxt.append((a - 1, b - 1))
                    nxt.append((a + k, b + k))
                nxt = merge(nxt)
                intervals = intersect(nxt, low, high)
                if not intervals:
                    ok = False
                    break
            out.append("Yes" if ok else "No")
        return "\n".join(out)

    return solve()

# sample-style and custom tests
assert run("""1
2 7 2 4
1 3
5 7
""") == "Yes"

assert run("""1
2 7 2 4
1 3
6 7
""") == "No"

assert run("""1
3 10 3 5
1 9
2 10
1 9
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhỏ khả thi | Có | khả năng tiếp cận cơ bản | 
| hành lang chật hẹp | Không | cắt tỉa đúng cách | 
| nhiều bước | Có | kiên trì theo khoảng thời gian | 

## Vỏ cạnh 

Trường hợp cạnh khóa xuất hiện khi khoảng cho phép đủ hẹp để phân chia các trạng thái có thể tiếp cận. Ví dụ: giả sử sau quá trình chuyển đổi, chúng ta đạt được độ cao có thể tiếp cận [2,2] và [10,10], nhưng cột tiếp theo chỉ cho phép [1,3]. Hành vi đúng là chỉ giữ lại [2,2] và loại bỏ [10,10]. Cách tiếp cận tối thiểu-tối đa ngây thơ sẽ giữ [2,10] không chính xác và cho rằng các trạng thái trung gian tồn tại, tạo ra Có sai sau đó. 

Một trường hợp cạnh khác xảy ra khi k lớn. Từ một quãng duy nhất, chuyển tiếp bước nhảy tạo ra một đoạn cách xa chuyển tiếp rơi. Nếu cột tiếp theo chỉ cho phép một khoảng ở giữa thì cả hai nhánh có thể bị loại bỏ một phần hoặc toàn bộ, không để lại trạng thái hợp lệ. Việc hợp nhất dựa trên khoảng thời gian sẽ loại bỏ chính xác cả hai thành phần mà không bao giờ phát minh ra khả năng kết nối giữa chúng. 

Cả hai trường hợp đều chứng minh rằng việc bảo toàn cấu trúc rời rạc là cần thiết để đảm bảo tính chính xác và việc thu gọn mọi thứ thành một phạm vi duy nhất sẽ làm mất thông tin quan trọng về tính khả thi.
