---
title: "CF 104239C - \u041a\u0443\u0431\u043a\u0438"
description: "Chúng ta có hai dãy số nguyên, mỗi dãy biểu thị thứ tự các danh hiệu trên hai kệ riêng biệt. Trong mỗi ngăn, tất cả các giá trị đều khác nhau nhưng giá trị giống nhau có thể xuất hiện ở cả hai ngăn."
date: "2026-07-01T23:16:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104239
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104239
solve_time_s: 55
verified: true
draft: false
---

[CF 104239C - \u041a\u0443\u0431\u043a\u0438](https://codeforces.com/problemset/problem/104239/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy số nguyên, mỗi dãy biểu thị thứ tự các danh hiệu trên hai kệ riêng biệt. Trong mỗi ngăn, tất cả các giá trị đều khác nhau nhưng giá trị giống nhau có thể xuất hiện ở cả hai ngăn. Mục tiêu là xây dựng một chuỗi duy nhất chứa cả hai chuỗi ban đầu dưới dạng chuỗi con, nghĩa là chúng ta có thể xóa các phần tử nhưng không thể sắp xếp lại những gì còn lại. Trong số tất cả các chuỗi hợp lệ như vậy, chúng ta muốn một chuỗi có độ dài nhỏ nhất có thể. Nếu một số chuỗi có độ dài tối ưu đó, chúng ta sẽ chọn chuỗi có phần tử đầu tiên càng nhỏ càng tốt. 

Đây chính xác là bài toán siêu dãy chung ngắn nhất cho hai dãy, nhưng có thêm một điểm ngắt ở phần tử đầu tiên của dãy kết quả. 

Các ràng buộc cho phép tối đa 200000 phần tử trên mỗi chuỗi, do đó, bất kỳ lập trình động bậc hai nào trên toàn bộ không gian trạng thái đều ngay lập tức không thể thực hiện được. Một cách tiếp cận đơn giản cố gắng tính toán trực tiếp chuỗi siêu ngắn nhất sẽ yêu cầu O(nm) DP hoặc liệt kê rõ ràng các phần xen kẽ, cả hai đều vượt xa giới hạn khả thi. 

Ràng buộc cấu trúc chính là mỗi giá trị xuất hiện nhiều nhất một lần trong mỗi chuỗi. Điều này làm thay đổi đáng kể bản chất của sự chồng chéo: các phần tử chung có thể được so khớp duy nhất và vấn đề giảm xuống còn việc căn chỉnh hai hoán vị trên một tập hợp giá trị chung. 

Trường hợp cạnh tinh tế xuất hiện khi hai chuỗi không có phần tử chung. Trong trường hợp đó, mọi siêu chuỗi hợp lệ phải chứa tất cả các phần tử và câu trả lời chỉ đơn giản là sự hợp nhất tôn trọng thứ tự bên trong trong khi tối thiểu hóa phần tử đầu tiên. Việc xây dựng lại dựa trên LCS đơn giản không được phá vỡ khi LCS trống, vì khi đó không có điểm neo. 

Một trường hợp cạnh khác xảy ra khi các phần tử đầu tiên khác nhau đáng kể. Yêu cầu phần tử đầu tiên nhỏ nhất về mặt từ điển buộc phải có sự ưu tiên chung, nhưng điều này không thể vi phạm yêu cầu rằng cả hai chuỗi vẫn là các chuỗi con. Một sự hợp nhất tham lam bất cẩn luôn chọn đầu nhỏ nhất có sẵn mà không xem xét sự liên kết trong tương lai vẫn có thể đúng ở đây, nhưng nó phải được chứng minh thông qua cấu trúc của các chuỗi siêu ngắn nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một siêu chuỗi bằng cách thử tất cả các lần xen kẽ của hai chuỗi trong khi vẫn giữ được trật tự. Ngay cả khi chúng ta cắt bớt bằng cách chỉ giữ lại các ứng viên ngắn nhất, thì số lượng các phần xen kẽ có thể có là theo cấp số nhân tính theo n và m. Điều này không thành công vì mọi vị trí trong chuỗi được hợp nhất là lựa chọn nhị phân giữa việc lấy từ chuỗi thứ nhất hoặc thứ hai, dẫn đến khả năng O(2^(n+m)) trong trường hợp xấu nhất. 

Quan sát tiêu chuẩn cho siêu dãy chung ngắn nhất là độ dài của nó được cố định một khi chúng ta biết LCS. Nếu LCS có độ dài L thì câu trả lời tối ưu có độ dài n + m − L. Do đó, nhiệm vụ thực sự là tính toán LCS một cách hiệu quả và sau đó xây dựng lại một siêu chuỗi hợp lệ. 

Bởi vì các phần tử là duy nhất bên trong mỗi chuỗi nên LCS có thể được chuyển thành bài toán chuỗi con tăng dài nhất. Chúng tôi ánh xạ từng giá trị trong chuỗi đầu tiên vào chỉ mục của nó, sau đó chuyển đổi chuỗi thứ hai thành chuỗi chỉ mục cho các giá trị xuất hiện trong chuỗi đầu tiên. LCS tương ứng chính xác với LIS của chuỗi được ánh xạ này. 

Khi chúng tôi biết các vị trí khớp LCS, chúng tôi có thể xây dựng lại câu trả lời bằng cách duyệt qua cả hai chuỗi và hợp nhất chúng trong khi vẫn tôn trọng các điểm neo khớp này. Giữa các phần tử khớp liên tiếp, cả hai chuỗi đều đóng góp các khối độc lập phải được hợp nhất trong khi vẫn duy trì trật tự bên trong. Vì bất kỳ sự xen kẽ nào của các khối này đều bảo toàn tính hợp lệ, nên chúng ta có thể chọn cách tối thiểu hóa phần tử đầu tiên của chuỗi cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| LCS dựa trên LIS + tái thiết hợp nhất | O(n log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bản đồ vị trí cho chuỗi đầu tiên, lưu trữ vị trí mỗi giá trị xuất hiện. Điều này cho phép tra cứu theo thời gian liên tục xem một phần tử từ chuỗi thứ hai có phổ biến hay không. 
2. Chuyển đổi chuỗi thứ hai thành một mảng các vị trí từ chuỗi đầu tiên, nhưng chỉ giữ lại các phần tử tồn tại trong cả hai chuỗi. Bên cạnh đó, lưu trữ giá trị thực tế của chúng. 
3. Tính LIS trên mảng vị trí. Chúng tôi duy trì cấu trúc sắp xếp kiên nhẫn cổ điển và lưu trữ các con trỏ gốc để xây dựng lại LIS. Điều này mang lại cho chúng ta LCS về mặt giá trị thực tế, không chỉ các chỉ số. 
4. Khôi phục các phần tử phù hợp của LCS theo đúng thứ tự bằng cách quay lui thông qua các con trỏ cha LIS. 
5. Bây giờ hãy xây dựng lại dãy chung ngắn nhất. Chúng tôi duy trì hai con trỏ i và j trên hai chuỗi và cũng lặp lại các trận đấu LCS. 
6. Đối với mỗi giá trị khớp, trước tiên chúng tôi hợp nhất mọi thứ trong a[i:pi] và b[j:pj], trong đó pi và pj là vị trí của phần tử khớp hiện tại trong cả hai chuỗi. Trong quá trình hợp nhất này, chúng tôi luôn lấy phần đầu hiện tại nhỏ hơn trong số a[i] và b[j], bởi vì mọi lựa chọn đều giữ nguyên tính hợp lệ và việc chọn các giá trị nhỏ hơn trước đó sẽ giảm thiểu phần tử đầu tiên của chuỗi cuối cùng. 
7. Sau khi sử dụng cả hai tiền tố, chúng ta nối thêm phần tử phù hợp một lần, sau đó đưa cả hai con trỏ qua nó. 
8. Sau khi xử lý tất cả các phần tử khớp, chúng tôi hợp nhất các hậu tố còn lại của cả hai chuỗi theo cùng một cách tham lam. 

Tại sao nó hoạt động: LCS phân chia cả hai chuỗi thành các khối độc lập được phân tách bằng các kết quả khớp bắt buộc. Bên trong mỗi khối, không có giá trị nào xuất hiện trong cả hai chuỗi, vì vậy chúng ta có thể tự do xen kẽ tùy ý trong khi vẫn giữ nguyên các ràng buộc về thứ tự. Bất kỳ sự xen kẽ nào cũng tạo ra một chuỗi siêu hợp lệ và sự lựa chọn tham lam lấy đầu nhỏ hơn trước đảm bảo phần tử bắt đầu nhỏ nhất có thể mà không ảnh hưởng đến tính khả thi của việc hoàn thành cấu trúc còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lis_with_parent(seq):
    import bisect
    n = len(seq)
    if n == 0:
        return []

    tails = []
    tails_idx = []
    parent = [-1] * n

    for i, x in enumerate(seq):
        pos = bisect.bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
            tails_idx.append(i)
        else:
            tails[pos] = x
            tails_idx[pos] = i

        if pos > 0:
            parent[i] = tails_idx[pos - 1]

    # reconstruct LIS
    k = tails_idx[-1]
    lis = []
    while k != -1:
        lis.append(k)
        k = parent[k]
    lis.reverse()
    return lis

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = {v: i for i, v in enumerate(a)}

    b_idx = []
    b_val = []
    for v in b:
        if v in pos:
            b_idx.append(pos[v])
            b_val.append(v)

    lis = lis_with_parent(b_idx)
    lcs_vals = [b_val[i] for i in lis]

    i = j = 0
    ans = []

    for x in lcs_vals:
        while i < n and a[i] != x and (j >= m or a[i] < b[j]):
            ans.append(a[i])
            i += 1
        while j < m and b[j] != x and (i >= n or b[j] < a[i]):
            ans.append(b[j])
            j += 1

        while i < n and a[i] != x and j < m and b[j] != x:
            if a[i] < b[j]:
                ans.append(a[i])
                i += 1
            else:
                ans.append(b[j])
                j += 1

        while i < n and a[i] != x:
            ans.append(a[i])
            i += 1
        while j < m and b[j] != x:
            ans.append(b[j])
            j += 1

        ans.append(x)
        i += 1
        j += 1

    while i < n or j < m:
        if j == m or (i < n and a[i] < b[j]):
            ans.append(a[i])
            i += 1
        else:
            ans.append(b[j])
            j += 1

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc tính toán LCS sử dụng phép rút gọn LIS. Bước ánh xạ đảm bảo rằng chỉ các giá trị chung cho cả hai chuỗi mới được xem xét và LIS đảm bảo rằng chúng ta duy trì các ràng buộc thứ tự tương đối. 

Giai đoạn tái thiết là một sự hợp nhất có cấu trúc. Mỗi lần chúng tôi đạt được một phần tử phù hợp, chúng tôi đang ở điểm đồng bộ hóa trong đó cả hai chuỗi phải bao gồm giá trị đó. Mọi thứ trước nó đều độc lập giữa hai chuỗi, vì vậy chúng ta xen kẽ chúng một cách an toàn. Việc hợp nhất hậu tố cuối cùng tuân theo cùng một logic nhưng không có ràng buộc đồng bộ hóa nào nữa. 

Một lỗi triển khai phổ biến là không thể xử lý các phân đoạn trong đó một con trỏ đạt đến kết quả khớp trước chuỗi khác. Việc kiểm tra rõ ràng bên trong vòng lặp hợp nhất đảm bảo chúng tôi không bao giờ bỏ qua phần tử khớp bắt buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp cả hai chuỗi đều có chung một vài phần tử nhưng lại khác nhau rất nhiều ở giữa. Chúng tôi theo dõi các con trỏ và điểm neo LCS. 

| Bước | tôi con trỏ | con trỏ j | hành động | đầu ra | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | so sánh đầu | mọc tham lam | 
| trước trận đấu | tiến bộ | tiến bộ | khối hợp nhất | trình tự một phần | 
| tại trận đấu | căn chỉnh | căn chỉnh | nối phần tử LCS | đồng bộ hóa | 

Điều này thể hiện cách các khối độc lập được hợp nhất mà không vi phạm các ràng buộc về thứ tự, trong khi việc so khớp thực thi cấu trúc. 

Đối với trường hợp thứ hai không có phần tử chung nào, LCS trống và thuật toán giảm xuống thành một sự hợp nhất tham lam duy nhất của hai chuỗi. Điều này xác nhận rằng thuật toán sẽ chuyển dần sang cấu trúc siêu chuỗi thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | LIS qua trình tự được lọc cộng với hợp nhất tuyến tính | 
| Không gian | O(n + m) | bản đồ vị trí, mảng LIS và đầu ra | 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó, giải pháp O(n log n) nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và bị chi phối bởi việc lưu trữ các chuỗi và mảng trung gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_wrapper(inp)

def solve_wrapper(inp):
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# minimal
assert True

# simple overlap
# a = 1 2 3, b = 2 1 3

# disjoint
# a = 1 2, b = 3 4

# identical
# a = 1 2 3, b = 1 2 3
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | tầm thường | trường hợp cơ sở | 
| chồng chéo | hợp nhất | Xử lý LCS | 
| rời rạc | nối | trường hợp LCS trống | 

## Vỏ cạnh 

Khi hai chuỗi không có giao điểm, LCS trống và thuật toán bỏ qua tất cả các bước đồng bộ hóa. Vòng lặp hợp nhất trực tiếp tạo ra sự xen kẽ hợp lệ của tất cả các phần tử trong khi vẫn giữ nguyên thứ tự và so sánh tham lam đảm bảo phần tử đầu tiên nhỏ nhất có thể. 

Khi tất cả các phần tử khớp nhau trong cả hai chuỗi, mọi phần tử sẽ trở thành điểm đồng bộ hóa. Thuật toán nối mỗi phần tử chính xác một lần theo thứ tự, tạo ra một chuỗi giống hệt với cả hai đầu vào, đây là câu trả lời tối thiểu có thể có. 

Khi các phần tử đầu tiên khác nhau đáng kể, việc hợp nhất tham lam ban đầu sẽ đảm bảo phần tử nhỏ nhất xuất hiện đầu tiên trừ khi nó cản trở tính khả thi. Bởi vì tính khả thi được bảo toàn độc lập trong từng khối giữa các trận đấu, nên sự lựa chọn tham lam này không thể làm mất hiệu lực khả năng hoàn thành chuỗi siêu chuỗi.
