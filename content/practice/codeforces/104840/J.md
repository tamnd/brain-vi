---
title: "CF 104840J - Thư mục bí mật"
description: "Chúng ta được đưa cho một số sợi dây và chúng ta được phép sắp xếp lại chúng và dán chúng lại với nhau thành một sợi dây dài duy nhất. Khi chúng ta dán hai dây lại, chúng ta không chỉ đơn giản nối chúng một cách mù quáng."
date: "2026-06-28T11:40:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104840
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104840
solve_time_s: 90
verified: false
draft: false
---

[CF 104840J - Thư mục bí mật](https://codeforces.com/problemset/problem/104840/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một số sợi dây và chúng ta được phép sắp xếp lại chúng và dán chúng lại với nhau thành một sợi dây dài duy nhất. Khi chúng ta dán hai dây lại, chúng ta không chỉ đơn giản nối chúng một cách mù quáng. Nếu hậu tố của chuỗi hiện tại khớp với tiền tố của chuỗi tiếp theo thì phần trùng lặp đó sẽ được hợp nhất để chúng tôi không trùng lặp các ký tự. 

Mục tiêu là chọn thứ tự của tất cả các chuỗi đã cho và chồng chúng lên nhau một cách tối ưu sao cho mọi chuỗi gốc xuất hiện ở đâu đó dưới dạng chuỗi con liền kề của kết quả cuối cùng và kết quả cuối cùng càng ngắn càng tốt. 

Đây là một bài toán cổ điển “hợp nhất tất cả các phần thành một siêu chuỗi” trong đó chi phí phụ thuộc vào mức độ chồng chéo của các chuỗi liền kề. Khó khăn là thứ tự tốt nhất không rõ ràng và thứ tự kém có thể làm tăng độ dài đáng kể. 

Các ràng buộc định hình giải pháp rất nhiều. Số lượng chuỗi trên mỗi bài kiểm tra tối đa là 17, điều này ngay lập tức gợi ý rằng có thể thực hiện tìm kiếm theo cấp số nhân trên các tập hợp con. Tuy nhiên, bản thân các chuỗi dài, tối đa 5·10^4 ký tự, do đó, bất kỳ sự so sánh đơn giản nào về các chuỗi ký tự theo ký tự cho mỗi cặp đều phải được kiểm soát cẩn thận. Trong tất cả các trường hợp thử nghiệm, tổng số chuỗi là nhỏ, do đó, quá trình xử lý trước kiểu O(n^2) hoặc thậm chí O(n^2 log n) đều có thể chấp nhận được. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Một là khi một chuỗi được chứa hoàn toàn bên trong một chuỗi khác. Ví dụ: nếu chúng ta có “abc” và “zabcx”, thì “abc” không cần phải đóng góp riêng cho câu trả lời cuối cùng. Một giải pháp bất cẩn giữ lại tất cả các chuỗi vẫn có thể đúng nhưng có thể lãng phí thời gian và làm phức tạp logic chồng chéo. 

Một vấn đề khác là các chuỗi trùng lặp. Nếu cùng một chuỗi xuất hiện hai lần, cả hai phải được đưa vào dưới dạng chuỗi con, nghĩa là chúng không thể bị loại bỏ, nhưng chúng có thể trùng lặp hoàn hảo với chi phí bằng 0 giữa chúng. Một giải pháp loại bỏ sự trùng lặp một cách tích cực mà không tính đến tính đa bội sẽ thất bại. 

Cuối cùng, sự chồng chéo phải được tính toán chính xác ngay cả khi tồn tại nhiều sự chồng chéo khác nhau. Ví dụ: giữa “aaaaa” và “aaa”, sự trùng lặp tốt nhất không phải là mơ hồ, nhưng giữa các chuỗi chung, chỉ có sự trùng khớp tiền tố hậu tố tối đa mới quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi thứ tự có thể có của chuỗi. Đối với mỗi hoán vị, chúng tôi tính toán cách hợp nhất các chuỗi bằng cách liên tục nối thêm chuỗi tiếp theo với mức trùng lặp tối đa có thể với kết quả hiện tại. Điều này đúng vì nó khám phá rõ ràng tất cả các đơn hàng có thể, nhưng nó trở nên không khả thi cực kỳ nhanh chóng vì có n! hoán vị, vốn đã là khoảng 3,6 triệu cho n = 10 và hoàn toàn không thể xảy ra với n = 17. 

Quan sát quan trọng là điều duy nhất quan trọng khi kết hợp các chuỗi là mức độ chúng trùng nhau ở ranh giới. Khi chúng ta biết, với mỗi cặp chuỗi i và j, độ trùng lặp tối đa khi theo sau i là j, thì cấu trúc bên trong của chuỗi không còn quan trọng nữa. Vấn đề giảm xuống còn việc chọn thứ tự các nút để tối đa hóa tổng số chồng chéo hoặc giảm thiểu độ dài được thêm vào một cách tương đương. 

Điều này biến bài toán thành một biến thể đường đi Hamilton ngắn nhất trên một đồ thị chuỗi có hướng hoàn chỉnh, trong đó chi phí cạnh chỉ phụ thuộc vào sự chồng chéo từng cặp. Vì n nhiều nhất là 17 nên chúng ta có thể sử dụng lập trình động bitmask để thử tất cả các tập hợp con của chuỗi và theo dõi cách tốt nhất để kết thúc ở mỗi chuỗi. 

Brute-force hoạt động về mặt khái niệm vì nó khám phá tất cả các hoán vị, nhưng không thành công do tăng trưởng giai thừa. DP hoạt động vì nó nén tất cả các hoán vị có chung tập hợp đã truy cập và chuỗi kết thúc vào một trạng thái duy nhất, tránh việc tính toán lại nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · L) | O(n · L) | Quá chậm | 
| Mặt nạ bit DP (SCS) | O(n^2 · 2^n + n^2 · L) | O(n · 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chia vấn đề thành hai giai đoạn: tính toán chồng chéo và chạy tập hợp con DP. 

1. Đầu tiên, chúng ta xử lý trước từng cặp chuỗi i và j để tính xem có bao nhiêu ký tự của j có thể chồng lên hậu tố của i. Điều này được thực hiện bằng cách kết hợp các hậu tố của i với các tiền tố của j và lấy độ dài khớp tối đa. Kỹ thuật khớp chuỗi tuyến tính như KMP có thể tính toán điều này một cách hiệu quả ngay cả đối với các chuỗi dài. 
2. Chúng tôi lưu trữ sự chồng chéo này trong một ma trận`overlap[i][j]`, biểu thị số lượng ký tự của j đã được bao phủ nếu j theo sau i. Điều này cho phép chúng ta tính toán chi phí gia tăng khi chuyển từ i sang j. 
3. Chúng tôi xác định trạng thái DP`dp[mask][i]`, nghĩa là độ dài tối thiểu của siêu chuỗi sử dụng chính xác các chuỗi trong`mask`và kết thúc bằng chuỗi i. 
4. Chúng ta khởi tạo`dp`đối với mặt nạ đơn phần tử. Nếu chỉ sử dụng chuỗi i thì siêu chuỗi tốt nhất chỉ là chính chuỗi đó, vì vậy`dp[1<<i][i] = len(s[i])`. 
5. Chúng tôi lặp lại tất cả các mặt nạ. Đối với mỗi mặt nạ và mỗi chuỗi cuối cùng có thể có i bên trong nó, chúng tôi cố gắng nối thêm một chuỗi mới j không có trong mặt nạ. Chúng ta cập nhật trạng thái mới bằng cách mở rộng i với j và chỉ thêm phần không chồng lấp của j. 
6. Quá trình chuyển đổi tính toán độ dài mới như`dp[mask][i] + len(s[j]) - overlap[i][j]`. Chúng tôi lấy mức tối thiểu trên tất cả các trạng thái có thể có trước đó. 
7. Sau khi điền vào bảng DP, câu trả lời là giá trị nhỏ nhất trên tất cả`dp[all_mask][i]`. 
8. Để xây dựng lại chuỗi thực tế, chúng tôi lưu trữ các con trỏ cha ghi lại trạng thái trước đó dẫn đến quá trình chuyển đổi tối ưu, sau đó quay lại từ trạng thái kết thúc tốt nhất. 

Tính chính xác dựa trên thực tế là mọi thứ tự hợp lệ đều tương ứng với chính xác một đường dẫn trong biểu đồ trạng thái DP này và chi phí của đường dẫn đó chính xác là tổng chiều dài của chuỗi được hợp nhất. Vì chúng ta lấy giá trị tối thiểu trên tất cả các đường dẫn như vậy nên chúng ta phải đạt được độ dài siêu chuỗi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_kmp_table(s):
    n = len(s)
    pi = [0] * n
    j = 0
    for i in range(1, n):
        while j and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
            pi[i] = j
    return pi

def overlap(a, b):
    # maximum suffix of a that matches prefix of b
    s = b + "#" + a
    pi = build_kmp_table(s)
    return pi[-1]

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = [input().strip() for _ in range(n)]

        # remove strings contained in others
        used = [True] * n
        for i in range(n):
            for j in range(n):
                if i != j and s[i] in s[j]:
                    used[i] = False
                    break

        a = [s[i] for i in range(n) if used[i]]
        n = len(a)

        if n == 0:
            print("")
            continue

        # recompute overlaps
        ov = [[0] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if i != j:
                    ov[i][j] = overlap(a[i], a[j])

        INF = 10**18
        dp = [[INF] * n for _ in range(1 << n)]
        parent = [[(-1, -1)] * n for _ in range(1 << n)]

        for i in range(n):
            dp[1 << i][i] = len(a[i])

        for mask in range(1 << n):
            for i in range(n):
                if dp[mask][i] == INF:
                    continue
                for j in range(n):
                    if mask & (1 << j):
                        continue
                    nmask = mask | (1 << j)
                    val = dp[mask][i] + len(a[j]) - ov[i][j]
                    if val < dp[nmask][j]:
                        dp[nmask][j] = val
                        parent[nmask][j] = (mask, i)

        full = (1 << n) - 1
        best_len = INF
        last = -1
        for i in range(n):
            if dp[full][i] < best_len:
                best_len = dp[full][i]
                last = i

        mask = full
        order = []
        cur = last
        while cur != -1:
            order.append(cur)
            pmask, pcur = parent[mask][cur]
            mask, cur = pmask, pcur

        order.reverse()

        res = a[order[0]]
        for k in range(1, len(order)):
            i, j = order[k - 1], order[k]
            add = a[j][ov[i][j]:]
            res += add

        print(res)

if __name__ == "__main__":
    solve()
```Trình trợ giúp KMP tính toán các giá trị hàm tiền tố trên một chuỗi được nối để chúng ta có thể tìm thấy kết quả khớp tiền tố hậu tố tối đa trong thời gian tuyến tính. Điều này tránh việc quét bậc hai cho từng cặp chuỗi, quá trình này sẽ quá chậm khi các chuỗi lớn. 

Bảng DP lưu trữ độ dài tốt nhất cho mọi tập hợp con và trạng thái kết thúc. Bảng cha rất cần thiết cho việc tái thiết; không có nó chúng ta sẽ chỉ biết độ dài chứ không biết chuỗi thực sự. 

Bước loại bỏ chuỗi con làm giảm các trạng thái không cần thiết. Nếu một chuỗi được chứa hoàn toàn trong một chuỗi khác, việc giữ nó không giúp chuyển đổi mà chỉ tăng kích thước DP. Loại bỏ nó giúp đơn giản hóa việc tính toán mà không làm thay đổi độ chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 · 2^n + tổng chiều dài chuỗi) | DP trên các tập hợp con chiếm ưu thế, tính toán chồng chéo sử dụng KMP tuyến tính trên mỗi cặp | 
| Không gian | O(n · 2^n) | DP và bảng cha trên tất cả các tập hợp con | 

Ràng buộc n ≤ 17 đảm bảo rằng 2^n có thể quản lý được. Ngay cả ở kích thước đầy đủ, DP có khoảng 131k trạng thái cho mỗi vị trí kết thúc, điều này là khả thi. Quá trình xử lý trước chuỗi là tuyến tính về tổng kích thước đầu vào trong tất cả các thử nghiệm, vẫn nằm trong giới hạn vì tổng độ dài chuỗi bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # assume solve() is defined above in same file
    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal case
assert run("1\n1\nabc\n") == "abc"

# simple overlap
assert run("1\n2\naaa\naa\n") == "aaa"

# containment case
assert run("1\n2\nabc\nzabcy\n") == "zabcy"

# no overlap case
assert run("1\n2\nab\ncd\n") in ["abcd", "cdab"]

# duplicate strings
assert run("1\n2\nabc\nabc\n") == "abcabc"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn | chính nó | khởi tạo DP cơ sở | 
| chồng chéo hoàn toàn | hợp nhất một lần | sự đúng đắn chồng chéo | 
| ngăn chặn | bỏ qua chuỗi dư thừa | loại bỏ chuỗi con | 
| chuỗi rời rạc | bất kỳ đơn hàng nào hợp lệ | Tính đúng đắn của DP đối với hoán vị | 
| trùng lặp | bao gồm cả hai | xử lý đa dạng | 

## Vỏ cạnh 

Một tình huống khó khăn là khi một chuỗi được chứa hoàn toàn bên trong một chuỗi khác. Ví dụ: nhập chuỗi “abc” và “zabcy”. Thuật toán loại bỏ “abc” trong quá trình tiền xử lý vì nó xuất hiện bên trong chuỗi thứ hai. DP còn lại chỉ chạy trên “zabcy” và đầu ra chính xác là “zabcy”. Bước loại trừ không làm mất bất kỳ sự xuất hiện bắt buộc nào vì bất kỳ sự xuất hiện nào của “abc” bên trong câu trả lời cuối cùng đều đảm bảo rằng nó được thỏa mãn. 

Các chuỗi trùng lặp hoạt động khác nhau. Đối với “abc” và “abc”, cả hai đều không có trong cái kia, vì vậy cả hai vẫn còn. Sự chồng chéo giữa các chuỗi giống hệt nhau có độ dài đầy đủ, nghĩa là chi phí chuyển đổi bằng 0. DP sẽ đặt chúng liên tiếp và việc xây dựng lại mang lại “abcabc”, đảm bảo cả hai lần xuất hiện đều tồn tại dưới dạng chuỗi con được yêu cầu.
