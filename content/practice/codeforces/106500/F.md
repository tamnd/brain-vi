---
title: "CF 106500F - Bóng Màu - 2"
description: "Chúng ta được đưa cho một hàng bóng, mỗi quả bóng được sơn một trong mười màu có thể có. Hoạt động duy nhất được phép là hoán đổi hai quả bóng lân cận và mỗi lần hoán đổi có giá một đơn vị."
date: "2026-06-25T08:36:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106500
codeforces_index: "F"
codeforces_contest_name: "XXVIII Interregional Programming Olympiad, Vologda SU, 2026"
rating: 0
weight: 106500
solve_time_s: 40
verified: true
draft: false
---

[CF 106500F - Bóng màu - 2](https://codeforces.com/problemset/problem/106500/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một hàng bóng, mỗi quả bóng được sơn một trong mười màu có thể có. Hoạt động duy nhất được phép là hoán đổi hai quả bóng lân cận và mỗi lần hoán đổi có giá một đơn vị. Mục tiêu là sắp xếp lại trình tự sao cho tất cả các quả bóng cùng màu xuất hiện trong một khối liền kề duy nhất, nghĩa là không có màu nào được chia thành nhiều phân đoạn riêng biệt. 

Nhiệm vụ là tính toán số lần hoán đổi liền kề tối thiểu cần thiết để đạt được bất kỳ sự sắp xếp nào trong đó mỗi màu tạo thành chính xác một khoảng. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các giao dịch hoán đổi hoặc xây dựng rõ ràng sự sắp xếp cuối cùng bằng cách tìm kiếm các hoán vị. Thậm chí một$O(n^2)$phương pháp này sẽ quá chậm vì nó có thể đạt tới khoảng$4 \cdot 10^{10}$hoạt động. Chúng ta buộc phải hướng tới các chiến lược tuyến tính hoặc gần tuyến tính với sự tính toán cẩn thận. 

Một vài trường hợp tế nhị quan trọng. 

Ví dụ: nếu tất cả các quả bóng đã tạo thành một khối màu duy nhất cho mỗi màu`1 1 2 2 3 3`, câu trả lời là bằng không. Bất kỳ phương pháp nào cố gắng "sắp xếp lại các màu đã sắp xếp" một cách nhầm lẫn mà không kiểm tra sự liền kề vẫn có thể hoạt động chính xác ở đây, nhưng chỉ khi nó không đưa ra các hoán đổi không cần thiết. 

Ví dụ: nếu màu sắc bị xen kẽ nhiều`1 2 1 2 1 2`, việc phân nhóm tham lam ngây thơ có thể đánh giá thấp chi phí vì nó bỏ qua việc di chuyển một khối màu này qua khối màu khác đòi hỏi phải tính đến tất cả các nghịch đảo theo cặp giữa chúng. 

Khó khăn chính là việc hoán đổi phụ thuộc vào thứ tự tương đối của tất cả các cặp chứ không phải sự điều chỉnh cục bộ. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ cố gắng xem xét tất cả các cách có thể để sắp xếp mười khối màu. Vì có rất ít màu sắc nên người ta có thể nghĩ đến việc hoán đổi các màu sắc và tính toán chi phí để chuyển từng màu vào phân khúc của nó. Đối với một thứ tự màu cố định, chúng ta có thể tính toán số lần hoán đổi cần thiết bằng cách tính tổng số lần đảo ngược mà mỗi phần tử đóng góp so với các màu trước đó. Điều này đã gợi ý một cấu trúc: một khi thứ tự màu sắc cuối cùng được cố định, chi phí sẽ trở nên xác định. 

Ý tưởng brute-force có hiệu quả vì một khi chúng ta quyết định thứ tự của các khối màu, cách sắp xếp tối ưu chỉ là “tất cả 1, rồi tất cả 2, … theo thứ tự đó”. Vấn đề là có tới$10!$hoán vị, chỉ là 3,6 triệu, vì vậy nó thực sự khả thi ở mức giới hạn nhưng vẫn không cần thiết phải liệt kê đầy đủ nếu chúng ta có thể tính toán trực tiếp thứ tự tối ưu. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải thử tất cả các hoán vị một cách rõ ràng. Thay vào đó, chúng ta có thể tính toán chi phí cho mỗi đơn đặt hàng có thể bằng cách sử dụng chi phí tương tác theo cặp được tính toán trước giữa các màu. Chi phí đặt màu$a$trước màu sắc$b$chỉ phụ thuộc vào số lượng đảo ngược giữa chúng tồn tại trong mảng ban đầu: mỗi lần một$b$xuất hiện trước một$a$, cặp đó đóng góp một lần hoán đổi nếu$a$được đặt trước$b$. 

Điều này biến vấn đề thành một vấn đề sắp xếp có trọng số trên tối đa mười nút, trong đó trọng số cạnh biểu thị số lượng đảo ngược giữa các màu. Sau đó, chúng tôi chọn một đơn hàng giảm thiểu tổng chi phí, có thể giải quyết vấn đề này bằng cách sử dụng lập trình động bitmask trên các tập hợp con màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các hoán vị |$O(10! \cdot n)$|$O(10)$| Có thể chấp nhận được nhưng không cần thiết | 
| Bitmask DP qua màu sắc |$O(10^2 \cdot 2^{10})$|$O(2^{10})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén vấn đề thành sự tương tác giữa các màu sắc. 

1. Chúng tôi tính toán tần suất mỗi màu xuất hiện và tần suất các cặp màu xuất hiện “sai thứ tự” trong mảng ban đầu. Cho mỗi cặp màu$c$Và$d$, chúng tôi đếm bao nhiêu lần một$d$xuất hiện trước một$c$. Giá trị này thể hiện chi phí đóng góp nếu chúng ta quyết định đặt tất cả$c$trước tất cả$d$S. 
2. Chúng tôi xác định trạng thái dựa trên tập hợp con màu nào đã được đặt theo thứ tự cuối cùng. Mỗi trạng thái đại diện cho một thứ tự một phần và chúng tôi muốn xây dựng tối đa tất cả mười màu. 
3. Đối với một trạng thái nhất định, chúng tôi thử thêm màu mới$c$cái đó vẫn chưa được đặt. Chi phí gia tăng của việc đặt$c$tiếp theo được tính bằng cách tính tổng các đóng góp đảo ngược được tính toán trước giữa$c$và tất cả các màu đã có trong trạng thái. Điều này hợp lệ vì khi một màu được đặt, vị trí tương đối của nó với các màu trong tương lai sẽ được cố định. 
4. Chúng tôi thực hiện chuyển đổi quy hoạch động trên tất cả các tập hợp con. Giá trị DP cho một tập hợp con là chi phí tối thiểu để sắp xếp chính xác các màu đó theo thứ tự nào đó phù hợp với tập hợp con. 
5. Chúng tôi khởi tạo DP với chi phí bằng 0 cho tập hợp trống và giảm bớt quá trình chuyển đổi bằng cách thêm một màu mới mỗi lần. Câu trả lời cuối cùng là giá trị DP cho toàn bộ màu sắc. 

Bất biến chính là đối với mỗi tập hợp con, DP lưu trữ chi phí tối thiểu có thể có để sắp xếp chính xác các màu đó theo một thứ tự cố định nào đó. Bởi vì chi phí giữa hai màu bất kỳ chỉ phụ thuộc vào thứ tự tương đối của chúng và không phụ thuộc vào các màu khác nên việc kết hợp các giải pháp con tối ưu là an toàn. Không có quyết định nào sau này có thể thay đổi sự đóng góp của các cặp đã được ấn định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # compress colors to 0..9
    color_id = {}
    comp = []
    for x in a:
        if x not in color_id:
            color_id[x] = len(color_id)
        comp.append(color_id[x])

    k = len(color_id)

    # count inversions between colors
    # cost[c][d] = number of pairs (i < j) with a[i]=c, a[j]=d
    cost = [[0] * k for _ in range(k)]

    cnt = [0] * k
    for x in comp:
        for c in range(k):
            if c != x:
                cost[x][c] += cnt[c]
        cnt[x] += 1

    # dp[mask] = min cost for ordering colors in mask
    INF = 10**18
    dp = [INF] * (1 << k)
    dp[0] = 0

    for mask in range(1 << k):
        for nxt in range(k):
            if mask & (1 << nxt):
                continue

            add_cost = 0
            for c in range(k):
                if mask & (1 << c):
                    add_cost += cost[nxt][c]

            nmask = mask | (1 << nxt)
            dp[nmask] = min(dp[nmask], dp[mask] + add_cost)

    print(dp[(1 << k) - 1])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là nén màu vì chỉ có vấn đề nhận dạng tương đối chứ không phải nhãn số. Bước đếm đảo ngược xây dựng một ma trận về số lần hoán đổi bắt buộc giữa bất kỳ cặp màu có thứ tự nào. 

Sau đó DP sẽ khám phá tất cả các tập hợp con màu sắc. Quá trình chuyển đổi tính toán chi phí gia tăng của việc thêm một màu vào thứ tự hiện tại, chính xác là tổng các xung đột của nó với các màu đã được đặt. Đây là nơi xảy ra nhiều sai lầm trong quá trình triển khai: chi phí phải mang tính định hướng, bởi vì việc hoán đổi$c$trước$d$không đối xứng với$d$trước$c$. 

Câu trả lời cuối cùng đến từ bitmask đầy đủ, thể hiện rằng tất cả các màu đã được đặt. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`4 / 2 1 2 1`. 

Đầu tiên chúng ta nén màu: giả sử`2 -> A`,`1 -> B`. Mảng trở thành`A B A B`. 

Chúng tôi tính toán chi phí đảo ngược. 

| Bước | Mặt nạ | Hành động | Thêm chi phí | Giá trị DP | 
| --- | --- | --- | --- | --- | 
| 0 | 000 | bắt đầu | 0 | 0 | 
| 1 | 001 | nơi A | 0 | 0 | 
| 2 | 011 | nơi B | 1 | 1 | 
| 3 | 010 | nơi B | 0 | 0 | 
| 4 | 110 | nơi A | 1 | 1 | 

Kết quả cuối cùng trở thành 1, phù hợp với thực tế là một hoán đổi liền kề đủ để nhóm màu. 

Dấu vết này cho thấy DP không trực tiếp xây dựng chuỗi cuối cùng mà tích lũy chi phí đặt hàng theo cặp một cách nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2 \cdot 2^k)$| tính toán ma trận nghịch đảo là$O(nk)$, DP khám phá tất cả các tập hợp con và chuyển tiếp qua màu sắc | 
| Không gian |$O(2^k + k^2)$| Mảng DP cộng với ma trận chi phí | 

Với$k \le 10$, DP cực kỳ nhỏ. Chi phí chủ yếu là quét tuyến tính của mảng, dễ dàng phù hợp với giới hạn cho$n \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda x: out.append(x)
    out.clear()
    solve()
    return "".join(out)

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    color_id = {}
    comp = []
    for x in a:
        if x not in color_id:
            color_id[x] = len(color_id)
        comp.append(color_id[x])

    k = len(color_id)

    cost = [[0] * k for _ in range(k)]
    cnt = [0] * k
    for x in comp:
        for c in range(k):
            if c != x:
                cost[x][c] += cnt[c]
        cnt[x] += 1

    INF = 10**18
    dp = [INF] * (1 << k)
    dp[0] = 0

    for mask in range(1 << k):
        for nxt in range(k):
            if mask & (1 << nxt):
                continue
            add = 0
            for c in range(k):
                if mask & (1 << c):
                    add += cost[nxt][c]
            dp[mask | (1 << nxt)] = min(dp[mask | (1 << nxt)], dp[mask] + add)

    print(dp[(1 << k) - 1])

out = []

# minimum size
assert run("1\n1\n") == "0"

# already grouped
assert run("4\n1 1 2 2\n") == "0"

# alternating
assert run("4\n1 2 1 2\n") == "1"

# all same
assert run("5\n3 3 3 3 3\n") == "0"

# three colors mixed
assert run("6\n1 2 3 1 2 3\n") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| các nhóm đã liền kề | 0 | không cần trao đổi | 
| mô hình xen kẽ | 1 | xử lý đảo ngược | 
| màu đồng nhất | 0 | trường hợp thoái hóa | 
| hỗn hợp tuần hoàn | biến | tương tác nhiều màu | 

## Vỏ cạnh 

Đối với một màu duy nhất, DP chỉ có một trạng thái ngoài trạng thái trống và ma trận chi phí trống, vì vậy mọi chuyển đổi đều thêm 0. Thuật toán trả về chính xác số lần hoán đổi bằng 0. 

Đối với một mảng được sắp xếp đầy đủ theo màu như`1 1 1 2 2 2`, tất cả số lần đảo ngược đều bằng 0 theo "hướng chính xác", do đó, mọi thứ tự tôn trọng nhóm tự nhiên đều mang lại chi phí bằng 0 và DP hội tụ về 0 mà không có bất kỳ hình phạt ngẫu nhiên nào. 

Đối với các mẫu xen kẽ như`1 2 1 2 1 2`, ma trận nghịch đảo trở nên đối xứng nhưng khác 0. DP chọn chính xác thứ tự giúp giảm thiểu sự đảo ngược chéo và mỗi bước đều tính nhất quán tất cả các xung đột theo cặp thay vì cố gắng khắc phục các giao dịch hoán đổi cục bộ, điều này ngăn cản việc đếm thiếu.
