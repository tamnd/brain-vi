---
title: "CF 2161A - Khứ hồi"
description: "Chúng tôi đang mô phỏng một chuỗi các vòng thi trong đó người tham gia đôi khi có thể điều chỉnh xếp hạng của họ trước mỗi vòng xếp hạng."
date: "2026-06-07T23:58:20+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2161
codeforces_index: "A"
codeforces_contest_name: "Pinely Round 5 (Div. 1 + Div. 2)"
rating: 800
weight: 2161
solve_time_s: 91
verified: true
draft: false
---

[CF 2161A - Chuyến đi khứ hồi](https://codeforces.com/problemset/problem/2161/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chuỗi các vòng thi trong đó người tham gia đôi khi có thể điều chỉnh xếp hạng của họ trước mỗi vòng xếp hạng. Điều phức tạp chính là việc một vòng có được xếp hạng cho người tham gia hay không phụ thuộc vào xếp hạng hiện tại của họ so với ngưỡng cố định và trong các vòng được xếp hạng, họ được phép thay đổi xếp hạng của mình trong một phạm vi giới hạn. 

Mỗi trường hợp thử nghiệm đưa ra xếp hạng ban đầu, ngưỡng phân tách hai loại vòng, kích thước bước tối đa để điều chỉnh mức xếp hạng trước vòng được xếp hạng và chuỗi các vòng được gắn nhãn là loại 1 hoặc loại 2. Vòng loại 1 luôn được xếp hạng. Vòng loại 2 chỉ được xếp hạng nếu xếp hạng hiện tại hoàn toàn dưới ngưỡng; nếu không nó sẽ không được xếp hạng và xếp hạng không thay đổi. 

Khi một vòng được xếp hạng, người chơi có thể chọn bất kỳ xếp hạng cuối cùng nào trong khoảng đối xứng xung quanh xếp hạng hiện tại. Điều này làm cho quá trình này giống như một bước đi được kiểm soát trên trục số, ngoại trừ một số bước bị “đóng băng” khi xếp hạng quá cao. 

Nhiệm vụ là tối đa hóa số vòng được xếp hạng. 

Các ràng buộc cho chúng ta biết rằng số vòng trên mỗi trường hợp thử nghiệm nhiều nhất là 1000 và tổng số trên tất cả các thử nghiệm nhiều nhất là 30000. Điều này ngay lập tức gợi ý rằng lập trình động bậc hai hoặc gần bậc hai trên tất cả các trạng thái và tất cả các vòng đều có thể chấp nhận được, nhưng bất kỳ khối nào trên 1000 trạng thái trên mỗi bước đều đã là ranh giới. Cần có một giải pháp chỉ duy trì một tập hợp nhỏ hoặc khoảng xếp hạng có thể tiếp cận trên mỗi bước. 

Khó khăn chính là về nguyên tắc, xếp hạng không bị giới hạn, nhưng chỉ mối quan hệ của nó với ngưỡng mới là quan trọng đối với các vòng loại 2. Điều này cho thấy rằng chúng ta không bao giờ cần theo dõi chính xác tất cả các xếp hạng, chỉ cần các phạm vi hoặc các khả năng cực đoan có thể ảnh hưởng đến việc chúng ta ở dưới hay trên ngưỡng. 

Một trường hợp thất bại tinh tế phát sinh khi các quyết định tham lam được thực hiện tại địa phương. Ví dụ: ngay lập tức đẩy xếp hạng xuống mức thấp nhất có thể giúp kích hoạt các vòng loại 2 trong tương lai, nhưng nó có thể làm giảm tính linh hoạt cho các vòng loại 1 khi chúng tôi có thể muốn duy trì trên ngưỡng để tránh những thay đổi trạng thái không mong muốn. Sự tương tác mang tính lâu dài nên các quyết định cục bộ không đáng tin cậy. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là coi mỗi vòng là phân nhánh trên tất cả các giá trị xếp hạng có thể đạt được sau mỗi vòng được xếp hạng. Sau một vòng xếp hạng, xếp hạng có thể trở thành bất kỳ số nguyên nào trong đoạn có độ dài 2D xung quanh xếp hạng hiện tại. Nếu chúng tôi theo dõi chính xác tất cả các xếp hạng có thể truy cập, chúng tôi sẽ nhanh chóng nhận được sự bùng nổ trạng thái: mỗi trạng thái mở rộng sang các trạng thái mới O(D) và qua n bước, điều này sẽ trở thành cấp số nhân trong n trong trường hợp xấu nhất. 

Tuy nhiên, cấu trúc của vấn đề khiến điều này trở nên quá mức cần thiết. Sự khác biệt có ý nghĩa duy nhất giữa các xếp hạng là liệu chúng có ở dưới X hay không, bởi vì điều đó quyết định liệu vòng loại 2 có hoạt động hay không. Bên trong khu vực bên dưới X và khu vực trên X, chúng tôi chỉ quan tâm đến khả năng tiếp cận của chúng tôi và liệu chúng tôi có thể vượt qua ngưỡng khi cần hay không. 

Thông tin chi tiết quan trọng là thay vì theo dõi tất cả xếp hạng có thể tiếp cận, chúng tôi theo dõi “vị trí hiệu quả” tốt nhất có thể để tối đa hóa các vòng xếp hạng trong tương lai. Chúng tôi nhận thấy rằng khi chúng tôi ở trong một vòng xếp hạng, chúng tôi luôn có thể di chuyển xếp hạng đến bất kỳ đâu trong khoảng kích thước 2D. Điều này có nghĩa là sau mỗi vòng xếp hạng, tập hợp các xếp hạng có thể có luôn là một khoảng liền kề nhau. Thuộc tính này thu gọn không gian trạng thái từ nhiều điểm thành một khoảng duy nhất.

Do đó, ở mỗi bước, chúng tôi đều duy trì xếp hạng tối thiểu và tối đa có thể đạt được sau khi tối đa hóa các vòng xếp hạng cho đến nay. Đối với mỗi vòng, chúng tôi cập nhật khoảng thời gian này tùy thuộc vào việc vòng đó có được xếp hạng hay không và loại 2 đang hoạt động hay bị bỏ qua. Khi vòng đấu được xếp hạng, chúng tôi mở rộng khoảng thời gian thêm D theo cả hai hướng. Khi nó bị bỏ qua, khoảng thời gian vẫn không thay đổi. Chúng tôi cũng cần tính đến thực tế là các vòng loại 2 chỉ được xếp hạng khi khoảng thời gian hiện tại giao nhau dưới X, điều này xác định liệu chúng tôi có đạt được điểm hay không và liệu chúng tôi có áp dụng chuyển đổi hay không. 

Điều này làm giảm vấn đề khi quét tuyến tính qua các vòng với các cập nhật theo khoảng thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các trạng thái xếp hạng) | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng thời gian DP | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các vòng từ trái sang phải trong khi vẫn duy trì phạm vi xếp hạng có thể có sau khi đạt được số vòng xếp hạng tối đa cho đến nay. 

1. Khởi tạo khoảng xếp hạng có thể truy cập hiện tại dưới dạng một điểm duy nhất ở xếp hạng ban đầu R₀. Đồng thời khởi tạo câu trả lời là 0. 
2. Quét từng vòng theo thứ tự. Đối với mỗi vòng, hãy xác định xem đó là loại 1 hay loại 2. 
3. Nếu vòng đấu thuộc loại 1, nó luôn được xếp hạng. Chúng tôi tăng câu trả lời lên 1. Sau đó, chúng tôi cập nhật khoảng bằng cách mở rộng khoảng đó thành [L - D, R + D], vì sau một vòng xếp hạng, chúng tôi có thể chọn bất kỳ xếp hạng nào trong D của bất kỳ giá trị nào có thể đạt được trước đó và kết hợp tất cả các khả năng sẽ mang lại một khoảng mở rộng đầy đủ. Bước này đảm bảo chúng ta bảo toàn được mọi khả năng trong tương lai. 
4. Nếu vòng là loại 2, trước tiên chúng tôi kiểm tra xem vòng này có thể được xếp hạng hay không, nghĩa là tồn tại ít nhất một xếp hạng trong khoảng hiện tại nhỏ hơn X. Điều này tương đương với việc kiểm tra xem giá trị tối thiểu của khoảng đó có nhỏ hơn X hay không. 
5. Nếu vòng loại 2 được xếp hạng theo khoảng thời gian hiện tại, chúng tôi sẽ tăng câu trả lời lên 1. Sau đó, chúng tôi cập nhật khoảng thời gian theo cách tương tự như đối với loại 1, mở rộng nó thêm D ở cả hai bên. 
6. Nếu vòng loại 2 không được xếp hạng, chúng tôi sẽ không thay đổi khoảng thời gian vì xếp hạng vẫn không thay đổi trong các vòng không được xếp hạng. 
7. Tiếp tục quá trình này cho đến khi tất cả các vòng được xử lý và xuất ra tổng số vòng được xếp hạng. 

Ý tưởng chính là chúng tôi luôn duy trì toàn bộ phạm vi xếp hạng có thể đạt được sau khi tối đa hóa các quyết định cho đến nay. Chúng ta không bao giờ cần chọn một quỹ đạo duy nhất vì khoảng thời gian đó đã thể hiện tất cả các khả năng tối ưu. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến là sau khi xử lý từng tiền tố của các vòng, khoảng [L, R] thể hiện chính xác tập hợp tất cả các xếp hạng có thể đạt được sau khi tối đa hóa số vòng được xếp hạng cho đến thời điểm đó. 

Khi một vòng được xếp hạng, mọi điểm trong khoảng trước đó có thể được di chuyển độc lập trong phạm vi ±D và sự kết hợp của tất cả các khoảng kết quả lại là một khoảng liên tục duy nhất. Khi một vòng không được xếp hạng, không có quá trình chuyển đổi nào xảy ra, do đó tập hợp có thể truy cập vẫn không thay đổi. Vì các vòng loại 2 chỉ được quyết định bằng việc liệu xếp hạng khả thi có nằm dưới X hay không, nên việc kiểm tra khoảng thời gian tối thiểu là đủ để quyết định xem vòng có thể được xếp hạng theo cách thực hiện tối ưu nào đó hay không. 

Bởi vì chúng tôi không bao giờ loại bỏ bất kỳ xếp hạng có thể truy cập nào có thể dẫn đến nhiều vòng xếp hạng hơn sau này nên số lượng cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        R0, X, D, n = map(int, input().split())
        s = input().strip()

        L = R0
        R = R0
        ans = 0

        for c in s:
            if c == '1':
                ans += 1
                L -= D
                R += D
            else:
                if L < X:
                    ans += 1
                    L -= D
                    R += D
                else:
                    pass

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giải thích khoảng thời gian. Các biến L và R đại diện cho toàn bộ phạm vi xếp hạng có thể có sau khi tối đa hóa các vòng xếp hạng. Đối với mỗi vòng loại 1, chúng tôi luôn mở rộng khoảng thời gian và tăng dần câu trả lời. 

Đối với vòng loại 2, điều kiện`L < X`là đủ vì nếu xếp hạng tối thiểu có thể đã cao hơn hoặc bằng X thì không có đường dẫn thực thi nào có thể làm cho xếp hạng đủ thấp để đủ điều kiện, vì khoảng đó chứa tất cả các trạng thái có thể truy cập. Sau khi vòng đấu được coi là đã xếp hạng, chúng tôi lại áp dụng quy tắc mở rộng tương tự. 

Một chi tiết tinh tế là chúng tôi không bao giờ cố gắng “ép buộc” xếp hạng vào một khu vực cụ thể; khoảng thời gian tự động mã hóa tất cả các lựa chọn hợp lệ. Điều này tránh bất kỳ việc quay lại hoặc phân tách trường hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
R0=2098, X=2100, D=5
s = 111211
```Chúng tôi theo dõi khoảng thời gian và trả lời: 

| Bước | Vòng | Khoảng [L, R] | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [2098, 2098] | đánh giá, mở rộng | 1 | 
| 2 | 1 | [2093, 2103] | đánh giá, mở rộng | 2 | 
| 3 | 1 | [2088, 2108] | đánh giá, mở rộng | 3 | 
| 4 | 2 | [2083, 2113] | đánh giá (L < X) | 4 | 
| 5 | 1 | [2078, 2118] | đánh giá, mở rộng | 5 | 
| 6 | 1 | [2073, 2123] | đánh giá, mở rộng | 6 | 

Điều này cho thấy khi khoảng thời gian giảm xuống dưới X, vòng loại 2 sẽ hoàn toàn có thể sử dụng được. 

### Ví dụ 2 

đầu vào:```
R0=2100, X=2100, D=5
s = 222
```| Bước | Vòng | Khoảng [L, R] | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [2100, 2100] | không được xếp hạng (L ≥ X) | 0 | 
| 2 | 2 | [2100, 2100] | không được xếp hạng | 0 | 
| 3 | 2 | [2100, 2100] | không được xếp hạng | 0 | 

Ở đây khoảng thời gian không bao giờ đạt đến dưới X, vì vậy loại 2 vòng không bao giờ kích hoạt. 

Những ví dụ này cho thấy rằng toàn bộ hành vi giảm xuống liệu khoảng thời gian có bao giờ giao nhau với “vùng đủ điều kiện” bên dưới X hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi vòng thực hiện cập nhật theo khoảng thời gian liên tục | 
| Không gian | O(1) | Chỉ có hai số nguyên được duy trì | 

Tổng số vòng trên tất cả các trường hợp thử nghiệm tối đa là 30000, do đó giải pháp chạy dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        R0, X, D, n = map(int, input().split())
        s = input().strip()

        L = R0
        R = R0
        ans = 0

        for c in s:
            if c == '1':
                ans += 1
                L -= D
                R += D
            else:
                if L < X:
                    ans += 1
                    L -= D
                    R += D

        out.append(str(ans))
    return "\n".join(out) + "\n"

# provided samples
assert run("""4
2100 2100 5 3
222
2098 2100 5 6
111211
2115 2100 226 7
2211121
0 10 4 8
22111121
""") == """0
6
5
8
"""

# minimum case
assert run("""1
0 1 1 1
1
""") == """1
"""

# all type 2 impossible
assert run("""1
100 50 10 3
222
""") == """0
"""

# all type 1
assert run("""1
5 100 2 4
1111
""") == """4
"""

# alternating stress
assert run("""1
10 10 3 6
121212
""") == """6
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn loại 1 | 1 | mở rộng cơ bản | 
| tất cả loại 2 bị chặn | 0 | ngưỡng không bao giờ đạt tới | 
| tất cả loại 1 | 4 | độ chính xác tăng trưởng lặp đi lặp lại | 
| mô hình xen kẽ | 6 | tương tác của cả hai quy tắc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi xếp hạng ban đầu đã ở dưới ngưỡng. Trong tình huống này, vòng loại 2 đầu tiên sẽ đủ điều kiện ngay lập tức và khoảng thời gian bắt đầu mở rộng ngay từ đầu. Thuật toán xử lý việc này một cách tự nhiên vì điều kiện`L < X`là đúng khi khởi tạo, do đó không cần vỏ đặc biệt. 

Một trường hợp khác là khi xếp hạng bắt đầu cao hơn X và D nhỏ. Trong những trường hợp như vậy, có thể mất vài vòng xếp hạng trước khi khoảng thời gian giảm xuống dưới X. Việc biểu diễn khoảng thời gian đảm bảo quá trình chuyển đổi này được nắm bắt một cách ngầm định khi các lần mở rộng lặp lại cuối cùng chỉ dịch chuyển giới hạn dưới xuống khi xếp hạng có thể được chọn tự do trong toàn bộ khoảng thời gian. 

Trường hợp cạnh cuối cùng là khi X cực kỳ lớn so với tất cả các xếp hạng có thể tiếp cận. Sau đó, không có vòng loại 2 nào được tính. Thuật toán giữ nguyên câu trả lời một cách chính xác vì khoảng không bao giờ giao nhau bên dưới X.
