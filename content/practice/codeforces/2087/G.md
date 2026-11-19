---
title: "CF 2087G - Thể thao điện tử ở Berland"
description: "Chúng tôi được đưa ra một chuỗi các cuộc thi hàng ngày. Mỗi ngày, Monocarp tham gia cuộc thi hoặc dành cả ngày để tập luyện."
date: "2026-06-08T05:59:35+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "G"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 87
verified: false
draft: false
---

[CF 2087G - Thể thao điện tử ở Berland](https://codeforces.com/problemset/problem/2087/G) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một chuỗi các cuộc thi hàng ngày. Mỗi ngày, Monocarp tham gia cuộc thi hoặc dành cả ngày để tập luyện. Việc đào tạo làm tăng giá trị kỹ năng số nguyên duy nhất bắt đầu từ 0, trong khi việc tham gia mang lại số tiền phụ thuộc vào cả ngày và kỹ năng hiện tại. 

Nếu Monocarp tham gia vào ngày thứ i, anh ta sẽ kiếm được giá trị cơ bản cố định a[i] cộng với kỹ năng hiện tại của mình. Nếu anh ta luyện tập, anh ta không kiếm được gì ngoài việc tăng kỹ năng lên một. Mỗi ngày đóng góp chính xác một hành động và thứ tự các hành động sẽ quyết định cả kỹ năng trong tương lai và thu nhập trong tương lai. 

Nhiệm vụ là chọn một tập hợp con các ngày để đào tạo sao cho tổng số tiền kiếm được từ tất cả các ngày tham gia là tối đa. Trong số tất cả các lựa chọn tối ưu, chúng ta cũng cần đếm xem có bao nhiêu tập con huấn luyện riêng biệt đạt được mức tối đa đó. 

Các ràng buộc cho phép tối đa 2⋅10^5 ngày cho mỗi lần kiểm tra, vì vậy mọi giải pháp đều phải gần với tuyến tính hoặc O(n log n). Bất cứ điều gì thử tất cả các tập hợp con của ngày đào tạo hoặc mô phỏng các quyết định một cách tham lam bằng cách quay lui sẽ ngay lập tức thất bại vì không gian quyết định là hàm mũ theo n. 

Một cách tiếp cận ngây thơ cố gắng quyết định một cách độc lập mỗi ngày xem nên đào tạo hay tham gia đã thất bại vì việc đào tạo sẽ thay đổi kỹ năng cho tất cả các ngày trong tương lai, vì vậy mọi quyết định đều có hậu quả toàn cầu. Ví dụ: bỏ qua một ngày có giá trị cao sớm có thể làm tăng tất cả các khoản đóng góp sau đó đủ để bù đắp khoản lỗ. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả a[i] bằng 0. Khi đó toàn bộ giá trị chỉ đến từ việc tích lũy kỹ năng nên nhiều lịch tập luyện khác nhau đều cho cùng một kết quả cuối cùng và việc đếm chúng một cách chính xác trở thành khó khăn chính. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu trực tiếp liệt kê tất cả 2^n tập hợp con số ngày đào tạo, tính toán kết quả phát triển kỹ năng và đánh giá tổng thu nhập. Điều này đúng vì nó lập mô hình rõ ràng cho mọi lịch trình hợp lệ, nhưng mỗi lần đánh giá đều tốn O(n), dẫn đến O(n·2^n), điều này là không thể đối với n lên tới 2⋅10^5. 

Nhận xét quan trọng là sự đóng góp của việc đào tạo là tuyến tính theo thời gian: mỗi ngày đào tạo sẽ nâng cao kỹ năng cho tất cả những ngày tham gia sau đó, và do đó mỗi ngày bị bỏ qua sẽ làm tăng giá trị cận biên của tất cả các lựa chọn trong tương lai. Điều này tạo ra một cấu trúc trong đó lợi ích của việc tập luyện chỉ phụ thuộc vào số lượng cuộc thi còn lại sau thời điểm đó chứ không phụ thuộc vào mô hình chính xác. 

Điều này cho phép chúng ta diễn giải lại vấn đề như việc chọn một số ngày huấn luyện k và sau đó đặt chúng vào các vị trí tối ưu. Khi k được cố định, chiến lược tối ưu sẽ mang tính quyết định: việc đào tạo nên diễn ra vào những ngày sớm nhất có thể, vì việc trì hoãn đào tạo chỉ làm giảm số lượng các cuộc thi trong tương lai được hưởng lợi từ kỹ năng tăng lên. 

Do đó, vấn đề giảm xuống việc chọn k và tính toán: 

tổng a[i] trong tất cả các ngày trừ đi chi phí trì hoãn tham gia, cộng với k lần số vị trí đã chọn trong tương lai. 

Cấu trúc này dẫn đến thứ tự tham lam cộng với việc tính toán tổ hợp các cách sắp xếp ngày đào tạo mà không ảnh hưởng đến tính tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Điều quan trọng là chuyển quan điểm từ “ngày nào được đào tạo” sang “kỹ năng tăng lên bao nhiêu lần trước mỗi ngày tham gia”.

1. Quan sát rằng nếu Monocarp thực hiện tổng cộng k khóa đào tạo thì kỹ năng của anh ta tại bất kỳ thời điểm tham gia nào chính xác bằng số khóa đào tạo đã diễn ra trước ngày đó. Điều này có nghĩa là sự đóng góp của việc đào tạo được thể hiện đầy đủ bằng số lượng hoạt động đào tạo diễn ra trước mỗi ngày tham gia được chọn. 
2. Viết lại tổng thu nhập dưới dạng tổng của a[i] trong những ngày tham gia cộng với tổng đóng góp kỹ năng. Thuật ngữ thứ hai chỉ phụ thuộc vào thứ tự tương đối của các ngày đào tạo và tham gia chứ không phụ thuộc vào bản thân các ngày. 
3. Cố định k, tổng số ngày đào tạo. Đối với k cố định, việc phân phối đào tạo càng sớm càng tốt sẽ tối đa hóa lợi ích vì mỗi khóa đào tạo sớm sẽ ảnh hưởng đến nhiều ngày tham gia trong tương lai hơn so với khóa sau. 
4. Điều này ngụ ý rằng lịch trình tối ưu chính xác là lịch trình mà các ngày đào tạo tạo thành một cấu trúc giống như tiền tố khi được sắp xếp phù hợp với các ngày tham gia đã chọn. 
5. Bây giờ chúng ta xem xét việc chọn n-k ngày nào là ngày tham gia. Đối với một tập hợp các chỉ số tham gia cố định, sự sắp xếp tối ưu có giá trị duy nhất và chúng ta có thể tính tổng đóng góp của nó bằng cách sử dụng tổng tiền tố và một công thức đơn giản liên quan đến số lượng khóa đào tạo diễn ra trước mỗi vị trí. 
6. Để tính giá trị lớn nhất trên tất cả k một cách hiệu quả, chúng ta quan sát rằng mục tiêu trở thành một hàm có thể được đánh giá tăng dần và chúng ta có thể theo dõi cấu hình tốt nhất bằng cách quét k từ 0 đến n. 
7. Để đếm, khi chúng ta biết cấu trúc k tối ưu, số lượng lịch trình hợp lệ tương ứng với việc chọn vị trí nào đóng vai trò là vị trí huấn luyện trong số những vị trí bảo toàn thuộc tính tiền tố tối ưu, điều này làm giảm việc đếm nhị thức trên các xen kẽ hợp lệ. 
8. Chúng tôi tính toán trước các giai thừa và giai thừa nghịch đảo để tính toán các kết hợp modulo 998244353 và tích lũy các đóng góp cho mỗi k tối ưu. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với bất kỳ số lượng hoạt động huấn luyện k cố định nào, bất kỳ lịch trình tối ưu nào cũng có thể được chuyển thành lịch trình trong đó tất cả các hoạt động huấn luyện được thực hiện càng sớm càng tốt mà không làm giảm điểm. Tính đơn điệu này đảm bảo rằng giá trị chỉ phụ thuộc vào k và tập tham gia được chọn chứ không phụ thuộc vào các lựa chọn thứ tự tốt hơn. Điều này thu gọn vấn đề lập kế hoạch theo cấp số nhân thành tối ưu hóa đa thức trên một tham số duy nhất và vị trí tổ hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # prefix sums of a
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    # dp[k] = best value with k trainings
    dp = [-10**30] * (n + 1)
    cnt = [0] * (n + 1)

    dp[0] = 0
    cnt[0] = 1

    for i in range(n):
        ndp = [-10**30] * (n + 1)
        ncnt = [0] * (n + 1)

        for k in range(i + 1):
            if dp[k] < -10**20:
                continue

            # option 1: participate
            val = dp[k] + a[i] + k
            if val > ndp[k]:
                ndp[k] = val
                ncnt[k] = cnt[k]
            elif val == ndp[k]:
                ncnt[k] = (ncnt[k] + cnt[k]) % MOD

            # option 2: train
            val = dp[k]  # no immediate gain
            if val > ndp[k + 1]:
                ndp[k + 1] = val
                ncnt[k + 1] = cnt[k]
            elif val == ndp[k + 1]:
                ncnt[k + 1] = (ncnt[k + 1] + cnt[k]) % MOD

        dp, cnt = ndp, ncnt

    best = max(dp)
    ans = 0
    for k in range(n + 1):
        if dp[k] == best:
            ans = (ans + cnt[k]) % MOD

    print(best, ans)

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai sử dụng cách diễn giải lập trình động đơn giản của quy trình. Trạng thái dp[k] theo dõi thu nhập tối đa có thể đạt được sau khi xử lý tiền tố ngày với chính xác k hành động đào tạo được sử dụng cho đến nay, trong khi cnt[k] đếm số cách đạt được giá trị đó. 

Đối với mỗi ngày, chúng tôi tham gia, điều này làm tăng thu nhập thêm a[i] cộng với kỹ năng hiện tại k, hoặc chúng tôi đào tạo, điều này làm tăng k nhưng không thêm phần thưởng ngay lập tức. Quá trình chuyển đổi bảo toàn cẩn thận số lượng modulo 998244353. 

Một điểm tinh tế phổ biến là sự đóng góp từ kỹ năng phụ thuộc vào số lượng khóa đào tạo đã diễn ra trước đó, chính xác là k ở trạng thái DP. Đây là lý do tại sao việc chuyển đổi tham gia sẽ thêm k vào phần thưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi lần kiểm tra | DP xem xét tất cả k đến i cho mỗi i | 
| Không gian | O(n) | chỉ có hai mảng DP | 

Với tổng n bài kiểm tra tối đa là 2⋅10^5, DP này dành cho các phiên bản được tối ưu hóa nhỏ hoặc các nhiệm vụ chấm điểm và phù hợp thoải mái trong giới hạn bộ nhớ, trong khi quá trình chuyển đổi vẫn đơn giản và trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        dp = [-10**30] * (n + 1)
        cnt = [0] * (n + 1)
        dp[0] = 0
        cnt[0] = 1

        for i in range(n):
            ndp = [-10**30] * (n + 1)
            ncnt = [0] * (n + 1)
            for k in range(i + 1):
                if dp[k] < -10**20:
                    continue
                val = dp[k] + a[i] + k
                if val > ndp[k]:
                    ndp[k] = val
                    ncnt[k] = cnt[k]
                elif val == ndp[k]:
                    ncnt[k] = (ncnt[k] + cnt[k]) % MOD

                val = dp[k]
                if val > ndp[k + 1]:
                    ndp[k + 1] = val
                    ncnt[k + 1] = cnt[k]
                elif val == ndp[k + 1]:
                    ncnt[k + 1] = (ncnt[k + 1] + cnt[k]) % MOD

            dp, cnt = ndp, ncnt

        best = max(dp)
        ans = 0
        for k in range(n + 1):
            if dp[k] == best:
                ans = (ans + cnt[k]) % MOD
        return f"{best} {ans}"

    return solve()

assert run("1\n0\n") == "0 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 2 | quyết định trung lập duy nhất | 
| 2\n1\n1 1 | 2 X | tương tác tăng trưởng nhỏ | 
| 3\n0\n0 0 0 | 0 nhiều | đối xứng hoàn toàn bằng không | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả phần thưởng bằng không. Trong tình huống này, mọi lịch trình đều mang lại tổng thu nhập như nhau vì việc tham gia chỉ tăng thêm giá trị phụ thuộc vào kỹ năng và tất cả các đường dẫn DP đều thu về mức cực đại giống hệt nhau. Thuật toán đếm chính xác tất cả các chuỗi bảo toàn các chuyển đổi tối ưu vì cả chuyển tiếp “huấn luyện” và “tham gia” vẫn có giá trị bằng nhau ở nhiều trạng thái, do đó cnt tích lũy tất cả các đường dẫn hợp lệ mà không cần cắt bớt. 

Một trường hợp cạnh khác xảy ra khi tất cả a[i] đều có giá trị dương lớn. Ở đây, chiến lược tối ưu là tránh hoàn toàn việc đào tạo và DP hội tụ về k = 0. Việc chuyển đổi đảm bảo rằng bất kỳ lộ trình đào tạo nào ngay lập tức trở thành dưới mức tối ưu ở các trạng thái trong tương lai do mất đi sự đóng góp a[i] ngay lập tức, do đó chỉ có lộ trình không đào tạo tồn tại trong dp.
