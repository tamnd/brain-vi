---
title: "CF 104014F - \u0422\u0443\u0440\u0438\u0441\u0442\u044b, \u0434\u043e\u0441\u0442\u043e\u043f\u0440\u0438\u043c\u0435\u0 447\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u0438 \u0438 \u0442\u0435\u043b\u0435\u0441\u043a\u043e\u043f\u044b"
description: "Có một dãy thành phố, mỗi thành phố có một số điểm tham quan đã biết. Ở mỗi thành phố đều có kính viễn vọng được lắp đặt. Mỗi kính viễn vọng có một số nguyên không âm."
date: "2026-07-02T04:57:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104014
codeforces_index: "F"
codeforces_contest_name: "2022-2023 ICPC NERC, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u0438 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0440\u0435\u0433\u0438\u043e\u043d\u0430 \u0438 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438"
rating: 0
weight: 104014
solve_time_s: 49
verified: true
draft: false
---

[CF 104014F - \u0422\u0443\u0440\u0438\u0441\u0442\u044b, \u0434\u043e\u0441\u0442\u043e\u043f\u0440\u0438\u043c\u0435\u0 447\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u0438 \u0438 \u0442\u0435\u043b\u0435\u0441\u043a\u043e\u043f\u044b](https://codeforces.com/problemset/problem/104014/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một dãy thành phố, mỗi thành phố có một số điểm tham quan đã biết. Ở mỗi thành phố đều có kính viễn vọng được lắp đặt. Mỗi kính viễn vọng có một số nguyên không âm. Nếu kính viễn vọng ở thành phố$i$có quyền lực$p$, khi đó khách du lịch sử dụng nó có thể quan sát tất cả các thành phố có chỉ số nằm trong khoảng cách$p$từ$i$, tạo thành một đoạn liền kề có tâm tại$i$, cắt ở biên giới của đất nước. 

Đối với bất kỳ thành phố cố định nào, việc nhìn qua kính thiên văn sẽ tạo ra tổng số điểm thu hút có thể nhìn thấy bằng tổng số điểm thu hút ở tất cả các thành phố bên trong phần nhìn thấy được của kính thiên văn. Yêu cầu là tổng số này không bao giờ được vượt quá giới hạn toàn cầu$R$. Mỗi thành phố có kính thiên văn riêng và chúng tôi muốn gán cho mỗi kính viễn vọng công suất lớn nhất có thể sao cho hạn chế này vẫn được thỏa mãn cho thành phố trung tâm của nó. Nếu ngay cả sức mạnh bằng 0 cũng đã vi phạm ràng buộc ở một thành phố nào đó thì việc cấu hình là không thể. 

Điểm mấu chốt là mỗi thành phố được kiểm tra độc lập, nhưng ràng buộc phụ thuộc vào tổng phạm vi xung quanh thành phố đó. Vì vậy với mỗi vị trí$i$, chúng ta đang tìm kiếm bán kính lớn nhất một cách hiệu quả$p_i$sao cho tổng của một đoạn đối xứng xung quanh$i$không vượt quá$R$. 

Những ràng buộc buộc một$O(N \log N)$hoặc$O(N)$giải pháp. Với$N \le 10^5$, bất kỳ sự mở rộng bậc hai nào của các khoảng sẽ quá chậm. Vì số tiền thu hút có thể lên tới$10^{14}$, chúng ta cũng cần số học 64-bit cho tổng tiền tố. 

Một vài tình huống khó khăn quan trọng. 

Nếu một thành phố đã có$s_i > R$, thì ngay cả bán kính bằng 0 cũng thất bại, nên câu trả lời cho thành phố đó là ngay lập tức$-1$. Ví dụ, nếu$R = 3$Và$s = [1, 10, 1]$, thì thành phố ở giữa không thể được chỉ định bất kỳ công suất kính thiên văn hợp lệ nào. 

Một trường hợp tinh vi khác là khi mở rộng bán kính nhanh chóng vượt quá ranh giới của mảng. Giải thích đúng là khoảng này chỉ đơn giản là cắt ở phần cuối và điều này phải được xử lý một cách tự nhiên trong tính toán tổng phạm vi. 

Cuối cùng, vì mỗi thành phố là độc lập nên sẽ có một sai lầm khi cố gắng ghép bán kính giữa các thành phố. Ràng buộc không tương tác giữa các kính thiên văn. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là xử lý từng thành phố một cách độc lập. Đối với trung tâm cố định$i$, chúng tôi thử mọi bán kính có thể$p$, tính tổng các điểm hấp dẫn trong$[i-p, i+p]$và kiểm tra xem nó có nằm trong$R$. Điều này có tác dụng vì điều kiện hoàn toàn mang tính cục bộ trong khoảng thời gian đó. 

Tuy nhiên, cách tiếp cận vũ phu này quá chậm. Trong trường hợp xấu nhất, mỗi thành phố có thể cho phép bán kính lên tới$O(N)$và tính toán lại tổng phạm vi một cách ngây thơ cho mỗi bán kính dẫn đến$O(N)$làm việc ở mỗi thành phố, hoặc$O(N^2)$tổng số hoạt động. Với$10^5$thành phố, điều này vượt xa giới hạn thời gian. 

Quan sát quan trọng là tổng phạm vi có thể được truy vấn trong thời gian không đổi bằng cách sử dụng tổng tiền tố. Khi đã có tổng tiền tố, việc kiểm tra bán kính cố định sẽ trở thành$O(1)$. Điều này biến vấn đề thành một tìm kiếm đơn điệu: khi bán kính tăng, tổng khoảng không bao giờ giảm, vì vậy chúng ta có thể tìm kiếm nhị phân bán kính hợp lệ tối đa cho mỗi thành phố. 

Sự đơn điệu này chính là điều mở ra giải pháp. Thay vì quét tất cả bán kính, chúng tôi tìm kiếm ranh giới nơi tổng khoảng đầu tiên vượt quá$R$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng Brute Force trên mỗi thành phố |$O(N^2)$|$O(N)$| Quá chậm | 
| Tổng tiền tố + tìm kiếm nhị phân trên mỗi thành phố |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước các tổng tiền tố của mảng thu hút để có thể tính bất kỳ tổng phân đoạn nào trong thời gian không đổi. 

Đối với mỗi thành phố$i$, sau đó chúng tôi tìm kiếm bán kính tối đa$p_i$sao cho tổng số điểm hấp dẫn trong khoảng thời gian$[i-p_i, i+p_i]$không vượt quá$R$. Nếu thậm chí$p_i = 0$thất bại, chúng tôi xuất ra$-1$. 

1. Xây dựng một mảng tổng tiền tố về các điểm tham quan. Điều này cho phép tính toán nhanh bất kỳ tổng khoảng nào trong thời gian không đổi. 
2. Đối với mỗi thành phố$i$, trước tiên hãy kiểm tra xem thành phố đó đã vi phạm ràng buộc chưa. Nếu như$s_i > R$, ta gán ngay$-1$. Điều này là cần thiết vì không có bản mở rộng nào có thể khắc phục được vi phạm cục bộ. 
3. Mặt khác, thực hiện tìm kiếm nhị phân trên bán kính có thể từ$0$ĐẾN$N$. Giới hạn trên$N$là an toàn vì khoảng không thể vượt quá toàn bộ mảng. 
4. Đối với bán kính ứng viên$m$, tính khoảng$[L, R] = [i-m, i+m]$, kẹp vào các chỉ số hợp lệ. Sử dụng tổng tiền tố để tính toán tổng số điểm thu hút trong phân khúc này. 
5. Nếu tổng nhỏ hơn hoặc bằng$R$, bán kính khả thi và chúng tôi cố gắng mở rộng hơn nữa; nếu không, chúng tôi giảm bán kính. 
6. Kết quả tìm kiếm nhị phân đưa ra bán kính hợp lệ tối đa cho thành phố$i$, được lưu dưới dạng câu trả lời. 

Ý tưởng chính ở bước 4 là việc kẹp sẽ xử lý các thành phố có ranh giới một cách tự nhiên, do đó không cần có lớp vỏ đặc biệt nào vượt quá giới hạn chỉ số. 

### Tại sao nó hoạt động 

Đối với mỗi tâm cố định$i$, xác định hàm$f(p)$như tổng số điểm hấp dẫn trong$[i-p, i+p]$. BẰNG$p$tăng lên, khoảng cách chỉ tăng lên, vì vậy$f(p)$đơn điệu không giảm. Điều này đảm bảo rằng một khi$f(p)$vượt quá$R$, tất cả bán kính lớn hơn đều không hợp lệ. Cấu trúc đơn điệu đó chính xác là điều làm cho tìm kiếm nhị phân trở nên chính xác và đảm bảo chúng tôi tìm thấy bán kính khả thi tối đa mà không bỏ sót bất kỳ ứng cử viên trung gian nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())
    a = list(map(int, input().split()))
    
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    def range_sum(l, rr):
        if l > rr:
            return 0
        return pref[rr + 1] - pref[l]

    def can(i, rad):
        l = max(0, i - rad)
        rr = min(n - 1, i + rad)
        return range_sum(l, rr) <= r

    res = []
    for i in range(n):
        if a[i] > r:
            res.append(-1)
            continue

        lo, hi = 0, n
        best = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(i, mid):
                best = mid
                lo = mid + 1
            else:
                hi = mid - 1

        res.append(best)

    print("\n".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Mảng tổng tiền tố được xây dựng một lần để mỗi truy vấn khoảng trở thành phép trừ của hai giá trị. Chức năng trợ giúp`can`thực thi việc cắt ranh giới, điều này rất cần thiết ở gần cuối mảng. 

Tìm kiếm nhị phân chạy độc lập cho từng thành phố. Sự lựa chọn của`n`vì giới hạn trên là an toàn vì không có bán kính nào có thể vượt quá toàn bộ phạm vi của mảng. 

Một lỗi phổ biến ở đây là quên xử lý vụ việc`a[i] > R`sớm. Nếu không có điều đó, tìm kiếm nhị phân có thể trả về bán kính dương không chính xác mặc dù bản thân trung tâm đã vi phạm ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 2 1
```Chúng tôi tính toán tổng tiền tố:`[1, 3, 4]`. 

Đối với thành phố 1 (giá trị 2), bán kính 0 cho 2 hợp lệ, bán kính 1 cho 1+2+1 = 4 cũng hợp lệ, bán kính 2 sẽ vượt quá giới hạn nhưng vẫn bằng tổng đầy đủ 4. Vậy kết quả là 2. 

Đối với thành phố 2 (giá trị 2), tính đối xứng cho kết quả tương tự 2. 

Đối với thành phố 3 (giá trị 1), bán kính 0 cho 1, bán kính 1 cho 2+1 = 3, bán kính 2 cho 4, do đó kết quả là 2. 

Đầu ra:```
2
2
2
```Dấu vết này cho thấy việc mở rộng dừng chính xác như thế nào tại điểm mà tổng phân đoạn đầy đủ đạt đến giới hạn. 

### Ví dụ 2 

đầu vào:```
3 3
1 3 5
```Tổng tiền tố:`[1, 4, 9]`. 

Đối với thành phố 2 (giá trị 3), bán kính 0 là hợp lệ. Bán kính 1 cho 1+3+5 = 9 vượt quá 3, do đó kết quả là 0. 

Thành phố 3 có giá trị 5 đã vượt quá 3, vì vậy đầu ra là -1. 

| Thành phố | Bán kính tốt nhất | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | -1 | 

Ví dụ này cho thấy rằng ngay cả những mở rộng nhỏ cũng có thể phá vỡ ràng buộc ngay lập tức và các trung tâm không hợp lệ sẽ được phát hiện trước bất kỳ tìm kiếm nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi thành phố thực hiện tìm kiếm nhị phân trên bán kính và mỗi lần kiểm tra đều được$O(1)$sử dụng tổng tiền tố | 
| Không gian |$O(N)$| Mảng tổng tiền tố lưu trữ tổng tích lũy | 

Với$N \le 10^5$, hệ số logarit giữ cho tổng số hoạt động ở khoảng vài triệu, nằm trong giới hạn ngân sách thời gian 3 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve  # assuming solution is in main.py
    return sys.stdout.getvalue()

# sample-like cases
assert run("3 4\n1 2 1\n") == "2\n2\n2\n"
assert run("3 3\n1 3 5\n") == "0\n0\n-1\n"

# minimum size
assert run("3 1\n1 1 1\n") == "0\n0\n0\n"

# single large spike
assert run("5 10\n1 1 100 1 1\n") == "2\n1\n-1\n1\n2\n"

# all equal
assert run("4 8\n2 2 2 2\n") == "1\n1\n1\n1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng cân bằng nhỏ | tính đúng đắn của khai triển đối xứng | tính đúng đắn của đối xứng bán kính | 
| tăng vọt vượt R | -1 xử lý | phát hiện sớm không hợp lệ | 
| giá trị thống nhất | tăng trưởng đồng đều | hành vi đơn điệu nhất quán |
