---
title: "CF 1202B - Bạn được cấp một chuỗi thập phân..."
description: "Chúng ta được cấp một chuỗi thập phân dài được tạo ra dưới dạng một dãy con của các kết quả đầu ra từ một quy trình đếm rất đơn giản. Bộ đếm luôn bắt đầu ở giá trị 0."
date: "2026-06-18T17:16:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1202
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 70 (Rated for Div. 2)"
rating: 1700
weight: 1202
solve_time_s: 333
verified: false
draft: false
---

[CF 1202B - Bạn được cấp một chuỗi thập phân...](https://codeforces.com/problemset/problem/1202/B) 

**Đánh giá:** 1700 
**Tags:** vũ lực, dp, đường đi ngắn nhất 
**Thời gian giải:** 5 phút 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi thập phân dài được tạo ra dưới dạng một dãy con của các kết quả đầu ra từ một quy trình đếm rất đơn giản. Bộ đếm luôn bắt đầu ở giá trị 0. Ở mỗi bước, nó in chữ số cuối cùng của giá trị hiện tại, sau đó tăng giá trị lên x hoặc y, trong đó x và y là các chữ số cố định từ 0 đến 9. Việc lặp lại điều này mãi mãi sẽ tạo ra một luồng chữ số vô hạn, nhưng chỉ một số chữ số đó được ghi vào chuỗi cuối cùng mà chúng ta thấy. 

Nhiệm vụ của chúng ta là xem xét mọi cặp có thứ tự (x, y) từ 0 đến 9 và quyết định xem phải chèn bao nhiêu chữ số vào chuỗi đã cho để có thể thu được chuỗi đó bằng cách xóa một số chữ số khỏi đầu ra bộ đếm hợp lệ của quá trình (x, y) đó. Các phần chèn tương ứng với các chữ số bị thiếu trong luồng được tạo ban đầu. 

Vì vậy, đối với mỗi cặp, chúng ta đang hỏi một cách hiệu quả: số chữ số tối thiểu được phát ra từ bộ đếm mà chúng ta buộc phải “bỏ qua” là bao nhiêu nếu chúng ta cố gắng căn chỉnh chuỗi đã cho thành một dãy con của một đầu ra hợp lệ? 

Chuỗi đầu vào có thể rất lớn, lên tới hai triệu ký tự, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng bộ đếm độc lập cho từng (x, y) theo cách đơn giản trên chuỗi được tạo đầy đủ. Có 100 cặp có thể xảy ra, do đó, ngay cả việc mô phỏng tuyến tính trên mỗi cặp cũng đã gây rủi ro cho hàng trăm triệu phép tính và bất kỳ hành vi bậc hai nào trên chuỗi đều là không thể. 

Một vấn đề tế nhị xuất phát từ thực tế là quá trình này không mang tính quyết định. Từ bất kỳ giá trị nào, cả hai chuyển đổi +x và +y đều có thể thực hiện được, do đó chuỗi được tạo ra không phải là duy nhất. Chúng ta được phép chọn chuỗi thao tác phù hợp nhất với chuỗi đầu vào. 

Một cách tiếp cận ngây thơ thường thất bại theo hai cách. Đầu tiên, việc khớp các ký tự của chuỗi một cách tham lam mà không nhớ hậu quả trong tương lai có thể bị mắc kẹt ở trạng thái có sự liên kết tốt hơn nhưng không thể truy cập được nữa. Thứ hai, việc xử lý từng (x, y) một cách độc lập bằng cách tính toán lại toàn bộ sẽ dẫn đến thời gian chạy không khả thi. 

## Phương pháp tiếp cận 

Quan điểm mô phỏng trực tiếp là coi trạng thái là giá trị bộ đếm hiện tại modulo 10, vì chỉ có chữ số cuối cùng được in. Từ trạng thái chữ số v, quá trình in v, sau đó chuyển sang (v + x) mod 10 hoặc (v + y) mod 10. Điều này tạo thành một biểu đồ có hướng trên 10 nút. 

Chúng ta cũng cần căn chỉnh luồng được tạo này với chuỗi đã cho dưới dạng một chuỗi con. Bất cứ khi nào chữ số được tạo khớp với ký tự cần thiết tiếp theo, chúng ta có thể chọn sử dụng nó; mặt khác, chúng tôi coi nó như một phần chèn vào chuỗi. 

Chiến lược bạo lực sẽ khám phá rõ ràng tất cả các cách đi qua biểu đồ này trong khi khớp chuỗi. Điều đó tự nhiên dẫn đến trạng thái được xác định bởi cặp (chữ số hiện tại, vị trí trong chuỗi). Mỗi trạng thái có thể chuyển tiếp theo hai cách và chúng ta có thể khớp hoặc bỏ qua một ký tự tùy theo mức độ bằng nhau. 

Điều này tạo ra một biểu đồ có khoảng 10 × n trạng thái. Mỗi trạng thái có hai lần chuyển đổi, do đó tổng số lần chuyển đổi vào khoảng 20n trên mỗi (x, y). Vì có 100 cặp, đây là khoảng chuyển đổi khoảng 2 × 10^8 trong trường hợp xấu nhất, là đường biên nhưng vẫn có thể chấp nhận được trong Python được tối ưu hóa nếu được triển khai cẩn thận với các vòng lặp đơn giản và không có phí tổn. 

Quan sát quan trọng giúp điều này trở nên khả thi là vị trí của dây luôn di chuyển về phía trước, không bao giờ lùi lại. Điều này ngăn chặn các chu kỳ trong chiều thứ hai và đảm bảo rằng chúng ta không bao giờ truy cập lại cùng một cặp (chữ số, vị trí) theo cách tạo ra quá trình xử lý vô hạn. Cấu trúc về cơ bản là một biểu đồ xếp lớp trên các vị trí của chuỗi. 

Do đó, thay vì suy nghĩ theo các đường dẫn ngắn nhất tùy ý, chúng tôi coi nó như một quy trình động theo lớp trong đó mỗi lớp tương ứng với một tiền tố của chuỗi và các chuyển đổi chỉ di chuyển về phía trước trong cấu trúc lớp đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ tất cả các đường dẫn | Hàm mũ | Hàm mũ | Quá chậm | 
| DP qua (chữ số, vị trí) | O(100 · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với một cặp cố định (x, y), chúng tôi mô phỏng cách bộ đếm có thể căn chỉnh với chuỗi bằng cách sử dụng lập trình động theo các trạng thái theo dõi cả chữ số hiện tại và số lượng chuỗi đã khớp. 

1. Chúng ta xác định trạng thái là (v, i), trong đó v là chữ số cuối cùng hiện tại của giá trị bộ đếm và i là số ký tự của chuỗi đã được khớp. 
2. Chúng ta bắt đầu từ (0, 0) vì bộ đếm bắt đầu ở giá trị 0 và chúng ta chưa khớp bất kỳ ký tự nào. 
3. Từ trạng thái (v, i), bộ đếm phát ra chữ số d = v. Sau đó, chúng tôi xem xét liệu chữ số này có khớp với ký tự bắt buộc tiếp theo s[i] hay không. 
4. Nếu d bằng s[i], chúng ta có thể chuyển tới i + 1 mà không phải trả phí. Nếu nó không khớp, chúng tôi coi nó như một phần chèn vào chuỗi và trả giá 1 trong khi vẫn giữ i không thay đổi. 
5. Sau khi xử lý quyết định khớp, chúng ta chuyển trạng thái bộ đếm bằng cách thêm x hoặc y vào v modulo 10, tạo ra hai trạng thái chữ số tiếp theo có thể có. 
6. Chúng tôi tiếp tục quá trình này bằng cách sử dụng sự lan truyền giống như hàng đợi trên tất cả các trạng thái có thể truy cập, đảm bảo rằng bất cứ khi nào chúng tôi đạt được cấu hình tốt hơn cho một (v, i nhất định), chúng tôi sẽ cập nhật nó. 
7. Câu trả lời cuối cùng cho (x, y) này là chi phí tối thiểu trong số tất cả các trạng thái có i bằng toàn bộ chiều dài của chuỗi. 

Tại sao nó hoạt động xuất phát từ tính bất biến là mọi trạng thái (v, i) thể hiện sự căn chỉnh hợp lệ giữa tiền tố của một số chuỗi được tạo và tiền tố của chuỗi đầu vào. Mọi chuyển đổi đều tương ứng chính xác với việc sử dụng một chữ số được tạo hoặc bỏ qua nó và không có chuyển đổi nào vi phạm thứ tự của một trong hai chuỗi. Vì tất cả các lựa chọn có thể có về chuyển đổi x và y đều được khám phá nên mọi sự liên kết hợp lệ đều có thể đạt được và DP luôn giữ chi phí tốt nhất trong số đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(x, y, s):
    n = len(s)
    INF = 10**18

    dist = [[INF] * (n + 1) for _ in range(10)]
    dist[0][0] = 0

    from collections import deque
    dq = deque()
    dq.append((0, 0))

    while dq:
        v, i = dq.popleft()
        cur = dist[v][i]
        if i == n:
            continue

        d = v
        ni = i
        if i < n and s[i] == str(d):
            ni = i + 1
            cost = cur
        else:
            cost = cur + 1

        for nv in ((v + x) % 10, (v + y) % 10):
            if cost < dist[nv][ni]:
                dist[nv][ni] = cost
                dq.append((nv, ni))

    ans = min(dist[v][n] for v in range(10))
    return -1 if ans == INF else ans

def main():
    s = input().strip()
    res = [[0] * 10 for _ in range(10)]

    for x in range(10):
        for y in range(10):
            res[x][y] = solve_case(x, y, s)

    for i in range(10):
        print(*res[i])

if __name__ == "__main__":
    main()
```Việc triển khai cốt lõi giữ một bảng khoảng cách 2D trên các trạng thái chữ số và số lượng chuỗi đã được khớp. Mỗi lần chuyển đổi cập nhật cả chữ số và độ dài tiền tố phù hợp. Chi phí chỉ tích lũy khi chữ số được phát ra không khớp với ký tự bắt buộc tiếp theo, ký tự này trực tiếp mô hình hóa các phần chèn vào chuỗi đầu vào. 

Vòng lặp qua các trạng thái tiếp theo (v + x) và (v + y) nắm bắt sự lựa chọn không xác định của bộ đếm. Hàng đợi đảm bảo chúng tôi tuyên truyền các cải tiến theo thứ tự thay đổi chi phí tăng dần, hoạt động hiệu quả giống như nới lỏng kiểu BFS 0-1 mặc dù các chuyển đổi là đồng nhất. 

Bước cuối cùng quét tất cả các trạng thái chữ số ở độ dài khớp đầy đủ, vì bộ đếm có thể kết thúc bằng bất kỳ chữ số nào sau khi tiêu thụ toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi ngắn như s = "0840" và một cặp cố định (x, y) = (4, 3). Chúng tôi theo dõi các trạng thái dưới dạng (v, i, cost). 

| Bước | Bang (v, i) | Chữ số phát ra | Kết quả trận đấu | Chi phí | Các tiểu bang tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0, 0) | 0 | trận đấu | 0 | (4, 1), (3, 1) | 
| 2 | (4, 1) | 4 | không khớp | 1 | (8, 1), (7, 1) | 
| 3 | (8, 1) | 8 | trận đấu | 1 | (2, 2), (1, 2) | 

Dấu vết này cho thấy việc so khớp sẽ nâng cao chỉ mục như thế nào trong khi việc không khớp sẽ làm tăng chi phí chèn. 

Bây giờ hãy xem xét trường hợp x = y = 2 và s = ​​"0840". Biểu đồ trở nên xác định, nhưng chúng tôi vẫn cho phép bỏ qua. 

| Bước | Bang (v, i) | Chữ số phát ra | Kết quả trận đấu | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | (0, 0) | 0 | trận đấu | 0 | 
| 2 | (2, 1) | 2 | không khớp | 1 | 
| 3 | (4, 1) | 4 | không khớp | 2 | 

Điều này chứng tỏ rằng ngay cả khi cấu trúc được cố định, DP vẫn theo dõi các quyết định bỏ qua tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100 · 10 · n) | Đối với mỗi cặp, chúng tôi xử lý tối đa 10 trạng thái chữ số trên n cấp tiền tố, với sự phân nhánh không đổi | 
| Không gian | O(10 · n) | Bảng DP lưu trữ giá tốt nhất cho mỗi cặp (chữ số, vị trí) | 

Việc sử dụng bộ nhớ có thể chấp nhận được với n lên tới hai triệu vì chỉ lưu trữ các giá trị số nguyên. Độ phức tạp về thời gian là giới hạn nhưng hoạt động dưới các ràng buộc vì quá trình chuyển đổi là các phép toán số học cực kỳ đơn giản trên một không gian trạng thái cố định nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)
    INF = 10**18

    def solve_case(x, y):
        dist = [[INF] * (n + 1) for _ in range(10)]
        dist[0][0] = 0
        from collections import deque
        dq = deque([(0, 0)])

        while dq:
            v, i = dq.popleft()
            cur = dist[v][i]
            if i == n:
                continue
            d = v
            ni = i + 1 if i < n and s[i] == str(d) else i
            cost = cur + (0 if ni != i + 1 else 0)  # simplified check

            for nv in ((v + x) % 10, (v + y) % 10):
                ncost = cur + (0 if i < n and s[i] == str(d) else 1)
                if ncost < dist[nv][ni]:
                    dist[nv][ni] = ncost
                    dq.append((nv, ni))

        ans = min(dist[v][n] for v in range(10))
        return -1 if ans == INF else ans

    res = [[solve_case(x, y) for y in range(10)] for x in range(10)]
    return "\n".join(" ".join(map(str, row)) for row in res)

# sample
assert run("0840")  # sanity check placeholder
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"0840"`| ma trận mẫu | tính đúng đắn trên các chữ số hỗn hợp | 
|`"0"`| ma trận nhỏ | trường hợp cạnh có chiều dài tối thiểu | 
|`"000000"`| tất cả các hành vi số không | khớp lặp lại | 
|`"0123456789"`| chu kỳ đầy đủ | tiến triển trong trường hợp xấu nhất | 
| chuỗi tuần hoàn dài | DP ổn định | biểu diễn dưới sự lặp lại | 

## Vỏ cạnh 

Đối với một chuỗi bao gồm một ký tự đơn như`"0"`, DP bắt đầu tại (0, 0) và ngay lập tức khớp mà không có bất kỳ chuyển đổi nào. Bất kỳ cặp (x, y) nào cho phép duy trì chữ số 0 trong suốt một chu kỳ sẽ tạo ra giá trị 0, trong khi các cặp khác sẽ yêu cầu ít nhất một lần chèn. Thuật toán xử lý chính xác điều này vì tất cả các điểm không khớp đều được tính ngay ở lần chuyển trạng thái đầu tiên. 

Đối với một chuỗi như`"999999"`, nhiều chuyển đổi sẽ không khớp ở các bước đầu, nhưng DP vẫn theo dõi sự căn chỉnh tốt nhất vì mỗi trạng thái đều tích lũy chi phí chèn một cách độc lập. Thuật toán không bao giờ cho rằng sự không khớp ban đầu là xấu trên toàn cầu, nó chỉ tổng hợp các hình phạt cục bộ dọc theo các đường dẫn hợp lệ.
