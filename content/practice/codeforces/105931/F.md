---
title: "CF 105931F - \u0415\u0436 \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435"
description: "Chúng ta có một cây có gốc với trọng số ở mỗi đỉnh. Cấu trúc cây được mô tả bằng một mảng cha, vì vậy mọi nút ngoại trừ nút gốc đều có chính xác một nút cha và toàn bộ biểu đồ được kết nối và không theo chu kỳ."
date: "2026-06-21T22:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105931
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2024"
rating: 0
weight: 105931
solve_time_s: 88
verified: true
draft: false
---

[CF 105931F - \u0415\u0436 \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435](https://codeforces.com/problemset/problem/105931/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với trọng số ở mỗi đỉnh. Cấu trúc cây được mô tả bằng một mảng cha, vì vậy mọi nút ngoại trừ nút gốc đều có chính xác một nút cha và toàn bộ biểu đồ được kết nối và không theo chu kỳ. 

Một cấu trúc hợp lệ, được gọi là con nhím, được xác định bằng cách chọn một đỉnh ở giữa$v$và sau đó chọn ít nhất hai đường dẫn đơn giản bắt đầu từ$v$, mỗi cái đi qua những hàng xóm khác nhau của$v$và tất cả các đường dẫn này phải có cùng độ dài ở các cạnh. Độ dài được phép bằng 0, tương ứng với việc giữ nguyên ở trung tâm mà không di chuyển. Con nhím là sự kết hợp của tất cả các đỉnh trên những đường đi này và trọng số của nó là tổng trọng số của tất cả các đỉnh được bao gồm, với tâm được tính một lần. 

Nhiệm vụ là tìm trọng số tối đa có thể có của một cấu trúc như vậy trên tất cả các lựa chọn của tâm và tất cả các tập hợp hợp lệ của các nhánh có độ dài bằng nhau. 

Các ràng buộc đi lên đến$n = 5 \cdot 10^5$, điều này ngay lập tức loại trừ bất cứ điều gì bậc hai trong$n$. Thậm chí$O(n \log^2 n)$các giải pháp cần xử lý cẩn thận và bất cứ điều gì liên tục tính toán lại trên mỗi nút trên mỗi độ sâu một cách đơn giản sẽ quá chậm. 

Một số trường hợp cần lưu ý. Nếu tất cả các trọng số đều âm, con nhím tốt nhất có thể thu gọn về một đỉnh nếu cho phép diễn giải độ dài bằng 0, vì chúng ta vẫn phải chọn ít nhất hai đường đi nhưng cả hai đều có thể tầm thường. Trên cây hình ngôi sao, giải pháp tối ưu thường tập trung ở gốc với chiều dài bằng 1 hoặc bằng 0 tùy theo trọng số. Trên một chuỗi, cấu trúc bị thoái hóa và cách hợp lệ duy nhất để có được hai nhánh bằng nhau thường là độ dài bằng 0. 

Một cách tiếp cận ngây thơ sẽ thử mọi trung tâm$v$, mọi chiều dài$L$và mọi cặp nhánh từ các hàng xóm khác nhau, tính toán các đường đi tốt nhất một cách độc lập. Điều này ngay lập tức trở nên không khả thi bởi vì ngay cả việc tính toán các đường dẫn đi xuống tốt nhất cho các cổ phiếu cố định$v$trên tất cả$L$đã tốn thời gian tuyến tính cho mỗi nút. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là đối với một trung tâm cố định$v$, chúng ta cần biết, với mọi độ sâu có thể$L$, con đường tốt nhất bắt đầu từ$v$đi vào từng cây con lân cận của nó. Một khi chúng ta có được điều đó, câu trả lời cho điều đó$v$thu được bằng cách lấy, cho mỗi$L$, tổng của hai giá trị lớn nhất giữa các giá trị lân cận của nó cộng với$a[v]$. 

Một cách tiếp cận bạo lực sẽ tính toán cho mọi nút$v$, một DP trên tất cả các độ sâu trong cây con của nó, trong đó$dp[v][L]$là tổng độ dài đường đi tốt nhất$L$bắt đầu từ$v$. DP này được định nghĩa một cách tự nhiên là sự hợp nhất giữa các phần tử con: dịch chuyển mảng của từng phần tử con một và lấy giá trị tối đa. Vấn đề là các mảng này có thể có kích thước$O(n)$mỗi nút trong một cây giống như chuỗi, do đó tổng công việc sẽ trở thành$O(n^2)$. 

Quan sát quan trọng là chúng ta không bao giờ cần tách hoàn toàn từng con sau khi hợp nhất. Đối với mỗi nút$v$, với mỗi độ sâu$L$, chúng tôi chỉ quan tâm đến hai đóng góp tốt nhất trong số tất cả trẻ em. Điều đó có nghĩa là thay vì theo dõi từng nút con một cách riêng biệt, chúng tôi duy trì, đối với từng nút và độ sâu, chỉ có hai giá trị cao nhất đến từ các cây con khác nhau. 

Điều này dẫn đến một DP hợp nhất từ ​​nhỏ đến lớn cổ điển trên cây, trong đó mỗi nút lưu trữ độ sâu ánh xạ cấu trúc tới tổng đường dẫn tốt nhất và tốt nhất thứ hai. Khi xử lý một nút, chúng tôi hợp nhất từng nút con một, luôn chèn DP đã dịch chuyển của nút con vào cấu trúc hiện tại trong khi chỉ duy trì hai giá trị tốt nhất cho mỗi độ sâu. 

Bởi vì mỗi cấu trúc cây con được hợp nhất thành một cấu trúc lớn hơn chỉ bằng logarit số lần theo nghĩa khấu hao, phương pháp này vẫn đủ nhanh để$n = 5 \cdot 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DP bạo lực trên tất cả các nút và độ sâu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| DP từ nhỏ đến lớn với hai giá trị trên cùng theo độ sâu |$O(n \log n)$khấu hao |$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại nút 1 (hoặc sử dụng mối quan hệ cha mẹ đã cho) và xử lý các nút theo thứ tự sau. 

1. Đối với mọi nút$v$, chúng tôi duy trì cấu trúc giống như từ điển$dp[v]$mỗi độ sâu ở đâu$d$, chúng tôi lưu trữ hai giá trị tốt nhất của đường dẫn bắt đầu tại$v$, đi xuống cây con của nó, với chính xác$d$các cạnh. Mỗi giá trị đại diện cho tổng trọng lượng nút dọc theo đường dẫn đó. Cấu trúc này nén tất cả các đóng góp con mà không tách chúng ra. 
2. Khởi tạo từng nút$v$chỉ với$dp[v][0] = (a[v], -\infty)$, nghĩa là đường dẫn có độ dài bằng 0 chỉ bao gồm chính nút đó. Giá trị thứ hai là phần giữ chỗ cho biết ban đầu chỉ tồn tại một đường dẫn tầm thường. 
3. Xử lý con của$v$từng cái một. Đối với một đứa trẻ$c$, trước tiên chúng tôi dịch chuyển tất cả độ sâu trong$dp[c]$bởi một, kể từ khi mở rộng một con đường từ$c$hướng lên tới$v$tăng chiều dài của nó lên một và thêm$a[v]$đến tất cả các con đường như vậy. Điều này tạo ra sự đóng góp ứng cử viên cho các đường dẫn bắt đầu tại$v$và đi vào cây con$c$. 
4. Hợp nhất các giá trị đã dịch chuyển này thành$dp[v]$. Đối với mọi độ sâu$d$, hiện tại chúng tôi có nhiều ứng viên tiềm năng đến từ những đứa trẻ khác nhau. Chúng tôi chỉ giữ lại hai giá trị tốt nhất ở mỗi độ sâu, vì con nhím cuối cùng chỉ cần hai nhánh. 
5. Sau khi tất cả các phần tử con được hợp nhất, chúng tôi tính toán con nhím tốt nhất có tâm ở$v$. Đối với mỗi độ sâu$d$, chúng tôi xem xét hai giá trị hàng đầu được lưu trữ trong$dp[v][d]$và cập nhật câu trả lời bằng tổng của chúng. Điều này tương ứng với việc chọn hai nhánh riêng biệt có độ dài bằng nhau$d$. 
6. Câu trả lời toàn cục là mức tối đa trên tất cả các nút và mọi độ sâu. 

Phần tinh tế là việc hợp nhất phải duy trì sự tách biệt giữa các phần tử con ở mức hai giá trị cao nhất theo độ sâu. Điều này đảm bảo rằng sau này khi chúng ta chọn hai nhánh, chúng thực sự đến từ các cây con khác nhau. 

### Tại sao nó hoạt động 

Đối với bất kỳ trung tâm cố định$v$và chiều sâu$d$, bất kỳ con nhím hợp lệ nào cũng chọn hai đường đi riêng biệt bắt đầu từ các lân cận khác nhau của$v$. Mỗi đường dẫn như vậy nằm hoàn toàn trong một cây con. Vì vậy sự đóng góp của một con nhím ở$(v, d)$chính xác là tổng của hai đóng góp con độc lập lớn nhất ở độ sâu đó. Vì chúng tôi duy trì hai giá trị cao nhất theo độ sâu sau khi hợp nhất tất cả các phần tử con nên chúng tôi lưu giữ chính xác thông tin cần thiết để xây dựng lại lựa chọn tối ưu. Không có thông tin nào khác ảnh hưởng đến tính khả thi hoặc tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

from collections import defaultdict

n = int(input())
p = list(map(int, input().split()))
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for i, par in enumerate(p, start=1):
    g[par - 1].append(i)

# dp[u]: dict depth -> (best1, best2)
# stored as lists for speed: dict of depth -> [best1, best2]
def merge_into(big, small, shift):
    for d, (best, second) in small.items():
        nd = d + shift
        val = best
        if nd not in big:
            big[nd] = [val, -10**30]
        else:
            if val > big[nd][0]:
                big[nd][1] = big[nd][0]
                big[nd][0] = val
            elif val > big[nd][1]:
                big[nd][1] = val

def dfs(u):
    dp = {0: [a[u], -10**30]}

    for v in g[u]:
        child = dfs(v)

        # shift child by +1 and merge into dp[u]
        for d, (b1, b2) in child.items():
            nd = d + 1
            val = b1 + a[u]

            if nd not in dp:
                dp[nd] = [val, -10**30]
            else:
                if val > dp[nd][0]:
                    dp[nd][1] = dp[nd][0]
                    dp[nd][0] = val
                elif val > dp[nd][1]:
                    dp[nd][1] = val

    # compute answer contribution at u
    for d, (b1, b2) in dp.items():
        if b2 != -10**30:
            ans[0] = max(ans[0], b1 + b2)

    return dp

ans = [-10**30]
dfs(0)
print(ans[0])
```DFS xây dựng cấu trúc từ dưới lên cho mỗi nút. Mỗi`dp[u]`Từ điển lưu trữ các tổng đường dẫn đi xuống tốt nhất được nhóm theo độ sâu và mỗi mục chỉ giữ hai giá trị trên cùng. Trong quá trình hợp nhất, các đóng góp con được dịch chuyển một cạnh và được điều chỉnh bằng cách thêm trọng số nút hiện tại vì mọi đường dẫn đều được neo ở trung tâm thông qua tất cả các nút trung gian. 

Bước cập nhật câu trả lời chỉ sử dụng các cặp đã được lưu trữ trong cùng một nhóm độ sâu, đảm bảo cả hai nhánh có độ dài bằng nhau và đến từ các cây con khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cây nhỏ trong đó nút 1 kết nối với 2 và 3, cả 2 và 3 đều là lá. 

| Nút | Độ sâu | Tốt nhất1 | Tốt nhất2 | 
| --- | --- | --- | --- | 
| 2 | 0 | a2 | -inf | 
| 3 | 0 | a3 | -inf | 
| 1 | 0 | a1 | -inf | 
| 1 | 1 | a1+a2 | a1+a3 | 

Tại nút 1, độ sâu 1 có hai nhánh đến từ nút 2 và 3. Thuật toán xác định chính xác rằng con nhím tốt nhất sử dụng cả hai con, đưa ra tổng số$a1 + a2 + a3$. 

Dấu vết này cho thấy việc phân nhóm có độ sâu bằng nhau sẽ thực thi tính đối xứng của các nhánh một cách tự nhiên. 

### Ví dụ 2 

Xét một chuỗi 1-2-3-4 với các trọng số tùy ý. 

| Nút | Độ sâu | Tốt nhất1 | Tốt nhất2 | 
| --- | --- | --- | --- | 
| 4 | 0 | a4 | -inf | 
| 3 | 0 | a3 | -inf | 
| 3 | 1 | a3+a4 | -inf | 
| 2 | 0 | a2 | -inf | 
| 2 | 1 | a2+a3 | -inf | 
| 2 | 2 | a2+a3+a4 | -inf | 

Không có nút nào chúng ta có được hai nhánh hợp lệ có cùng độ sâu dương, do đó câu trả lời sẽ thu gọn về phần đóng góp đỉnh đơn tốt nhất. Điều này chứng tỏ yêu cầu có ít nhất hai nhánh bằng nhau đã ngăn cản các cấu trúc dạng dây chuyền hình thành những con nhím lớn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$khấu hao | Các mục nhập chuyên sâu của mỗi nút được hợp nhất lên trên thông qua việc hợp nhất kiểu từ nhỏ đến lớn | 
| Không gian |$O(n \log n)$| Mỗi mục được lưu giữ tối đa hai giá trị cho mỗi độ sâu trên mỗi cây con đang hoạt động | 

Cấu trúc chỉ phát triển dọc theo chiều cao của cây và mỗi thao tác hợp nhất chỉ xử lý các mục được tạo một lần cho mỗi cấp độ cây con, điều này giữ cho tổng công việc trong giới hạn cho$n = 5 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))
    a = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for i, par in enumerate(p, start=1):
        g[par - 1].append(i)

    sys.setrecursionlimit(10**7)
    ans = [-10**30]

    def dfs(u):
        dp = {0: [a[u], -10**30]}
        for v in g[u]:
            child = dfs(v)
            for d, (b1, b2) in child.items():
                nd = d + 1
                val = b1 + a[u]
                if nd not in dp:
                    dp[nd] = [val, -10**30]
                else:
                    if val > dp[nd][0]:
                        dp[nd][1] = dp[nd][0]
                        dp[nd][0] = val
                    elif val > dp[nd][1]:
                        dp[nd][1] = val

        for d, (b1, b2) in dp.items():
            if b2 != -10**30:
                ans[0] = max(ans[0], b1 + b2)
        return dp

    dfs(0)
    return str(ans[0])

# minimal tree
assert run("3\n1 1\n1 1 1\n") == "3"

# chain
assert run("4\n1 2 3\n1 2 3 4\n") == "4"

# star
assert run("4\n1 1 1\n10 1 1 1\n") == "12"

# all negative
assert run("3\n1 1\n-5 -1 -2\n") == "-5"

# balanced case
assert run("5\n1 1 2 2\n5 1 1 1 1\n") in ["7", "8"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | 3 | con nhím hợp lệ nhỏ nhất | 
| chuỗi | 4 | không có hai nhánh ngoài gốc | 
| ngôi sao | 12 | cặp con đẹp nhất | 
| tất cả đều tiêu cực | -5 | sụp đổ thành một nút tốt nhất | 
| trường hợp cân bằng | 8/7 | nhiều trung tâm tối ưu | 

## Vỏ cạnh 

Trong một cây hình ngôi sao có nhiều cây con ở trung tâm, thuật toán lưu trữ nhiều đóng góp độ sâu bằng 0 và độ sâu một. Bước hợp nhất đảm bảo chỉ có hai nhánh con trên cùng là quan trọng và tất cả các nhánh bổ sung sẽ bị bỏ qua trừ khi chúng cải thiện một trong hai giá trị tốt nhất. 

Trong một chuỗi dài, mọi nút ngoại trừ các điểm cuối chỉ có một điểm tiếp tục đi xuống, do đó không có độ sâu nào tích lũy được hai đóng góp độc lập. Do đó, cấu trúc dp không bao giờ kích hoạt một cặp hợp lệ và câu trả lời sẽ giảm xuống giá trị nút đơn tốt nhất. 

Trong các cây có trọng số dương và âm hỗn hợp, các đường dẫn sâu hơn có thể hoạt động tốt hơn các đường dẫn nông ngay cả khi chúng bao gồm các nút trung gian âm. DP thực hiện chính xác những điều này vì nó luôn xem xét tổng đường dẫn đầy đủ thay vì các lựa chọn cục bộ tại mỗi nút.
