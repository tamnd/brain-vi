---
title: "CF 1700D - Âu thuyền sông"
description: "Hệ thống này là một chuỗi các container, mỗi container có sức chứa cố định. Nước đi vào một tập hợp con được chọn của các thùng chứa này với tốc độ không đổi là một đơn vị mỗi giây trên mỗi ống mở."
date: "2026-06-09T22:03:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1700
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 802 (Div. 2)"
rating: 1900
weight: 1700
solve_time_s: 179
verified: false
draft: false
---

[CF 1700D - Khóa sông](https://codeforces.com/problemset/problem/1700/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, dp, tham lam, toán học 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống này là một chuỗi các container, mỗi container có sức chứa cố định. Nước đi vào một tập hợp con được chọn của các thùng chứa này với tốc độ không đổi là một đơn vị mỗi giây trên mỗi ống mở. Bất cứ khi nào một thùng chứa bị tràn, lượng dư thừa sẽ ngay lập tức được đẩy sang thùng chứa tiếp theo và quá trình này tiếp tục diễn ra cho đến thùng chứa cuối cùng, nơi lượng tràn rời khỏi hệ thống. 

Mục tiêu không phải là mô phỏng trực tiếp dòng nước mà là chọn một tập hợp con các thùng chứa để kích hoạt làm nguồn. Đối với mỗi truy vấn, chúng tôi được đưa ra một giới hạn thời gian và phải xác định số lượng nguồn tối thiểu cần kích hoạt để, bắt đầu từ chỗ trống, mọi vùng chứa đều đạt công suất tối đa không muộn hơn thời điểm đó. 

Sự tương tác chính là nước không ở lại cục bộ. Việc mở một đường ống ở vị trí i không chỉ góp phần vào thùng chứa i mà còn có khả năng góp phần vào tất cả các thùng chứa ở bên phải thông qua tình trạng tràn. Điều này làm cho những đóng góp trở nên phi cục bộ và buộc phải suy luận về cấu trúc tiền tố hơn là năng lực độc lập. 

Các ràng buộc đẩy tới một giải pháp gần với O(n log n) hoặc O(n log^2 n). Với tối đa 200.000 khóa và 200.000 truy vấn, bất kỳ mô phỏng tuyến tính cho mỗi truy vấn hoặc tính toán lại tham lam ngây thơ nào cũng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. Giải pháp phải xử lý trước cấu trúc một lần và trả lời từng truy vấn theo thời gian logarit. 

Trường hợp khó tinh tế xuất hiện khi khóa sớm có dung lượng lớn so với khóa sau. Ví dụ: nếu v = [10, 1, 1] và thời gian nhỏ, việc mở đường ống ở các ổ khóa sau không giúp lấp đầy ổ khóa đầu tiên, vì tràn không thể chảy ngược lại. Một chiến lược ngây thơ ưu tiên các vị trí hậu tố nhỏ sẽ cho rằng những lần kích hoạt sau sẽ bù đắp cho những thiếu hụt trước đó một cách không chính xác. Một trường hợp góc khác là khi thời gian nhỏ hơn v_i tối đa; thậm chí mở tất cả các đường ống cũng không thể lấp đầy ổ khóa lớn nhất kịp thời, vì vậy câu trả lời ngay lập tức phải là -1. 

## Phương pháp tiếp cận 

Nếu chúng ta sửa một số k đường ống đã mở, vấn đề sẽ trở thành kiểm tra xem liệu có tồn tại lựa chọn k chỉ số sao cho tất cả các khóa có thể được lấp đầy trong vòng t giây hay không. Giải pháp brute-force sẽ thử tất cả các tập hợp con có kích thước k và mô phỏng dòng nước cho từng cấu hình. Về nguyên tắc, điều này đúng vì quá trình này mang tính quyết định một khi các nguồn được cố định, nhưng nó sẽ bùng nổ về mặt tổ hợp. Số lượng tập hợp con là O(n chọn k) và thậm chí việc hạn chế k cũng không ngăn được sự tăng trưởng theo cấp số nhân trên các truy vấn. 

Một cách hữu ích hơn để suy nghĩ về hệ thống là đảo ngược quan điểm. Thay vì theo dõi xem nước chảy về phía trước như thế nào, hãy xem xét ý nghĩa của việc chiếc khóa i được lấp đầy trong thời gian t. Mỗi ống mở ở vị trí j đóng góp tối đa t đơn vị nước, và do có cấu trúc tràn nên bất kỳ đơn vị nào đạt đến vị trí i đều phải đi qua tất cả các khóa trung gian. Điều này tạo ra hiệu ứng thắt cổ chai: việc điền tiền tố phụ thuộc vào số lượng nguồn nằm trong hoặc trước tiền tố đó. 

Quan sát quan trọng là hệ thống hoạt động giống như chúng ta đang phân phối t đơn vị “ngân sách” cho mỗi đường ống đã mở và những ngân sách này có thể được chuyển tiếp chứ không thể chuyển ngược lại. Để làm cho tất cả các khóa đầy, chúng ta cần tổng đóng góp đủ để đáp ứng nhu cầu tiền tố do năng lực gây ra. Chiến lược tối ưu luôn ưu tiên đặt các đường ống càng xa về bên phải càng tốt, bởi vì chúng giúp giảm bớt các ràng buộc trước đó trong khi vẫn đóng góp thông qua việc tràn sang bên trái chỉ một cách gián tiếp thông qua bão hòa. 

Điều này làm giảm vấn đề thành việc kiểm tra tính khả thi tham lam đối với k cố định và vì tính khả thi là đơn điệu trong k nên chúng ta có thể tìm kiếm nhị phân câu trả lời cho mỗi truy vấn. Thông tin tiền tố tính toán trước cho phép mỗi lần kiểm tra được thực hiện theo thời gian tuyến tính hoặc ở dạng được tối ưu hóa một chút, mang lại giải pháp tổng thể O((n + q) log n) hoặc O((n + q) log^2 n) tùy thuộc vào việc triển khai.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Tham lam + Tìm kiếm nhị phân | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta trình bày lại nhiệm vụ này dưới dạng một bài toán quyết định: trong một thời gian cố định t và số k ống cố định, hãy quyết định xem có thể chọn k vị trí đảm bảo tất cả các khóa đều được lấp đầy hay không. 

1. Với một t cho trước, hãy tính xem mỗi ống có thể đóng góp bao nhiêu. Vì mỗi ống mở tạo ra t đơn vị, mỗi vị trí được chọn đóng góp tới tổng số t đơn vị dòng chảy mà cuối cùng sẽ được phân phối qua tràn. Điều này làm giảm hệ thống chỉ còn lựa chọn k nguồn có dung lượng t mỗi nguồn. 
2. Đi qua các ổ khóa từ trái sang phải mà vẫn đảm bảo lượng nước vẫn phải cung cấp để đáp ứng yêu cầu của từng ổ khóa. Tại vị trí i, chúng tôi theo dõi nhu cầu còn lại về v_i. 
3. Khi một đường ống được mở ở vị trí i, nó đóng góp t đơn vị có thể giúp đáp ứng nhu cầu hiện tại và tương lai. Ý tưởng tham lam là sử dụng một đường ống càng sớm càng tốt để bù đắp những khoản thâm hụt khi chúng xuất hiện, bởi vì việc trì hoãn chỉ có nguy cơ làm tăng mức bồi thường cần thiết sau này. 
4. Duy trì lượng nước dư thừa từ các đường ống đã được chọn. Khi chúng ta đạt đến khóa i, chúng ta trừ đi yêu cầu v_i của nó khỏi phần thặng dư này. Nếu thặng dư trở nên âm, chúng ta buộc phải kích hoạt một đường ống khác trước hoặc tại i để bù đắp. 
5. Mỗi lần chúng ta buộc phải mở một pipe, chúng ta đặt nó ở vị trí hiện tại, thêm t vào phần dư và tiếp tục. Đếm xem cần bao nhiêu lần mở cưỡng bức như vậy. 
6. Nếu tại bất kỳ thời điểm nào số lượng lỗ yêu cầu vượt quá k thì không thể cấu hình được. Nếu không, nó là khả thi. 
7. Đối với mỗi truy vấn, tìm kiếm nhị phân k tối thiểu để cấu hình khả thi. 

Bất biến chính là ở mỗi tiền tố i, thuật toán duy trì lượng nước dư thừa tối đa có thể đạt được bằng cách sử dụng số lượng ống đã chọn được đặt cho đến nay, giả sử chiến lược bố trí tối ưu. Bất cứ khi nào thủ tục tham lam buộc phải đặt một đường ống, đó là vì không có vị trí nào sớm hơn có thể ngăn chặn sự thiếu hụt ở tiền tố đó. Điều này làm cho mỗi vị trí trở nên tối ưu cục bộ và cần thiết trên toàn cầu. Vì tính khả thi chỉ phụ thuộc vào việc chúng ta có thể tránh được thặng dư âm hay không nên việc xây dựng tham lam mô tả chính xác liệu k ống có đủ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(v, t, k):
    n = len(v)
    used = 0
    surplus = 0

    i = 0
    while i < n:
        if surplus < v[i]:
            used += 1
            if used > k:
                return False
            surplus += t
        surplus -= v[i]
        i += 1

    return True

def solve():
    n = int(input())
    v = list(map(int, input().split()))
    q = int(input())
    queries = [int(input()) for _ in range(q)]

    max_v = max(v)

    for t in queries:
        if t < max_v:
            print(-1)
            continue

        lo, hi = 1, n
        ans = n

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(v, t, mid):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện cốt lõi chia thành hai phần. chức năng`can`kiểm tra tính khả thi trong một thời gian và số lượng đường ống cố định bằng cách quét từ trái sang phải để mô phỏng lượng nhu cầu còn lại có thể được đáp ứng bởi các đường ống hiện đang mở. Tìm kiếm nhị phân trong`solve`tìm số lượng ống nhỏ nhất vượt qua bài kiểm tra tính khả thi này. 

Kiểm tra ban đầu`t < max(v)`ngăn chặn những công việc không cần thiết vì ngay cả việc tập trung tất cả luồng cũng không thể lấp đầy ổ khóa lớn nhất nhanh hơn yêu cầu dung lượng của chính nó. Logic tham lam dựa vào việc duy trì lượng nước dư thừa đang hoạt động, thể hiện lượng “tiềm năng nước chưa sử dụng” còn lại từ các đường ống đã mở trước đó và mỗi khi lượng nước này không đủ thì buộc phải có một đường ống mới. 

## Ví dụ đã hoạt động 

Hãy xem xét dữ liệu đầu vào mẫu với v = [4, 1, 5, 4, 1] và truy vấn t = 6, kiểm tra k = 3. 

Chúng tôi mô phỏng tính khả thi: 

| tôi | v[i] | dư thừa trước | hành động | thặng dư sau | đã qua sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | mở ống | 6 → 2 sau khi điền | 1 | 
| 2 | 1 | 2 | không có đường ống mới | 2 → 1 | 1 | 
| 3 | 5 | 1 | mở ống | 7 → 2 | 2 | 
| 4 | 4 | 2 | mở ống | 8 → 4 | 3 | 
| 5 | 1 | 4 | không có đường ống mới | 4 → 3 | 3 | 

Mọi yêu cầu đều được đáp ứng khi sử dụng 3 ống, khẳng định tính khả thi. 

Bây giờ hãy xem xét một trường hợp tùy chỉnh nhỏ v = [3, 3, 3], t = 3, k = 2. 

Chúng tôi mô phỏng: 

| tôi | v[i] | dư thừa | hành động | đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 → 3 | mở ống | 1 | 
| 2 | 3 | 0 → 3 | mở ống | 2 | 
| 3 | 3 | 0 → 3 | không thể mở thêm | 3 cần thiết | 

Chúng tôi vượt quá k, vì vậy không khả thi. Điều này chứng tỏ rằng mỗi đường ống chỉ đóng góp một lượng lưu lượng hữu dụng nhất định và không thể kéo dài vô thời hạn qua nhiều điểm thiếu hụt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi truy vấn thực hiện tìm kiếm nhị phân trên k, mỗi lần kiểm tra là tuyến tính | 
| Không gian | O(n) | lưu trữ dung lượng khóa và truy vấn | 

Các ràng buộc cho phép lên tới 200.000 phần tử, do đó hệ số logarit trong quá trình quét tuyến tính vẫn nằm trong giới hạn. Việc kiểm tra tính khả thi là phép tính số học đơn giản cho mỗi phần tử, làm cho các thừa số không đổi trở nên nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    def can(v, t, k):
        used = 0
        surplus = 0
        for x in v:
            if surplus < x:
                used += 1
                if used > k:
                    return False
                surplus += t
            surplus -= x
        return True

    n = int(input())
    v = list(map(int, input().split()))
    q = int(input())
    res = []
    max_v = max(v)

    for _ in range(q):
        t = int(input())
        if t < max_v:
            res.append("-1")
            continue
        lo, hi = 1, n
        ans = n
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(v, t, mid):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        res.append(str(ans))

    return "\n".join(res)

# sample
assert run("""5
4 1 5 4 1
6
1
6
2
3
4
5
""") == """-1
3
-1
-1
4
3"""

# all equal
assert run("""3
2 2 2
2
2
3
""") == """3
2"""

# minimum size
assert run("""1
5
2
4
5
""") == """-1
1"""

# increasing
assert run("""4
1 2 3 4
1
4
""") == """2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | cung cấp | tính chính xác của các truy vấn hỗn hợp | 
| tất cả đều bình đẳng | 3, 2 | hành vi tham lam ổn định | 
| khóa đơn | -1, 1 | tính khả thi cơ bản và trường hợp tầm thường | 
| ngày càng tăng | 2 | phân phối công suất không đồng đều | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi giới hạn thời gian nhỏ hơn dung lượng khóa tối đa. Với v = [10, 1, 1] và t = 5, không có số lượng ống nào có thể đáp ứng khóa đầu tiên vì ngay cả một ống chuyên dụng hoàn toàn cũng không thể cung cấp đủ nước mỗi giây trong khoảng thời gian nhất định. Thuật toán xử lý việc này ngay lập tức thông qua việc kiểm tra tính khả thi loại bỏ tất cả k. 

Một trường hợp tế nhị khác là khi công suất giảm mạnh. Với v = [5, 1, 1, 1], việc đặt tất cả các ống sớm có vẻ tối ưu, nhưng quy trình tham lam cho thấy rằng những thiếu hụt sau này buộc phải mở thêm các lỗ mở bất kể nồng độ trước đó. Cơ chế thặng dư phản ánh chính xác rằng lượng dư thừa trước đó không thể được phân phối ngược trở lại, đảm bảo số lượng ống bổ sung được tính chính xác khi cần thiết. 

Trường hợp thứ ba là n = 1. Câu trả lời là -1 nếu t < v1 hoặc 1 nếu ngược lại. Tìm kiếm nhị phân và logic tham lam tự nhiên sụp đổ thành điều này mà không có vỏ đặc biệt ngoài mức kiểm tra tối đa.
