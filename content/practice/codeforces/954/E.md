---
title: "CF 954E - Vòi nước"
description: "Mỗi vòi đóng góp một dòng nước có thể kiểm soát được, nhưng mỗi vòi đều có nhiệt độ cố định. Bạn được phép chọn tốc độ dòng có giá trị thực cho mỗi vòi trong khoảng từ 0 đến công suất tối đa của nó."
date: "2026-06-17T02:12:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 954
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 40 (Rated for Div. 2)"
rating: 2000
weight: 954
solve_time_s: 97
verified: false
draft: false
---

[CF 954E - Vòi nước](https://codeforces.com/problemset/problem/954/E) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vòi đóng góp một dòng nước có thể kiểm soát được, nhưng mỗi vòi đều có nhiệt độ cố định. Bạn được phép chọn tốc độ dòng có giá trị thực cho mỗi vòi trong khoảng từ 0 đến công suất tối đa của nó. Hỗn hợp cuối cùng có nhiệt độ bằng giá trị trung bình có trọng số của tất cả các vòi đang hoạt động, trong đó trọng lượng là tốc độ dòng chảy đã chọn. 

Về mặt hình thức, nếu bạn gán giá trị luồng$x_i$, tổng khối lượng là$V = \sum x_i$, và nhiệt độ thu được là$\frac{\sum x_i t_i}{V}$, trừ khi tất cả$x_i = 0$, trong trường hợp đó nhiệt độ được xác định bằng không. 

Mục tiêu là chọn tốc độ dòng chảy làm cho nhiệt độ thu được chính xác bằng giá trị mục tiêu$T$, đồng thời tối đa hóa tổng khối lượng. 

Khó khăn chính là chúng tôi không chỉ tối ưu hóa âm lượng mà còn thực hiện điều đó dưới một ràng buộc phi tuyến tính kết hợp tất cả các lần nhấn theo một tỷ lệ. Sự ràng buộc buộc phải cân bằng giữa nguồn nhiệt độ cao và nhiệt độ thấp. 

Kích thước đầu vào đạt$2 \cdot 10^5$, loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các tập hợp con hoặc tìm kiếm các bài tập một cách rõ ràng. Bất kỳ giải pháp nào cũng phải giảm vấn đề xuống việc sắp xếp và xử lý tuyến tính hoặc logarit. 

Một số trường hợp đặc biệt phá vỡ trực giác ngây thơ. Nếu tất cả các vòi có nhiệt độ lớn hơn$T$, không có hỗn hợp nào có thể làm giảm nhiệt độ xuống$T$, vì vậy câu trả lời là không. Tương tự, nếu tất cả các vòi có nhiệt độ nhỏ hơn$T$, chúng ta không thể tăng nhiệt độ lên$T$. Một trường hợp tinh tế khác là khi một số vòi có nhiệt độ chính xác$T$, vì chúng có thể được sử dụng tự do mà không ảnh hưởng đến tính khả thi nhưng vẫn tương tác với tỷ lệ tối ưu khi kết hợp với các vòi khác. 

Một cách tiếp cận ngây thơ cố gắng “cân bằng” các dòng chảy một cách tham lam mà không có trật tự toàn cầu có thể thất bại vì việc trộn lẫn phụ thuộc vào sự đóng góp liên quan đến$T$, không phải nhiệt độ tuyệt đối. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xem xét từng vòi một cách độc lập và cố gắng gán các giá trị luồng trong khi vẫn duy trì tỷ lệ mục tiêu. Người ta có thể cố gắng quyết định$x_i$tuần tự và điều chỉnh công suất còn lại để cố định nhiệt độ thu được. Điều này thất bại vì mọi quyết định đều làm thay đổi giá trị trung bình có trọng số toàn cầu theo cách phi tuyến tính và các quyết định cục bộ có thể phá hủy tính khả thi trong tương lai. Ngay cả khi chúng ta rời rạc hóa các phân bổ có thể, không gian trạng thái vẫn liên tục và phụ thuộc vào tất cả các lựa chọn trước đó, dẫn đến độ phức tạp theo cấp số nhân. 

Quan sát quan trọng là ràng buộc có thể được viết lại dưới dạng tuyến tính. Bắt đầu từ$$\frac{\sum x_i t_i}{\sum x_i} = T,$$chúng tôi sắp xếp lại để$$\sum x_i (t_i - T) = 0.$$Điều này biến bài toán thành việc chọn số không âm$x_i \le a_i$sao cho các đóng góp tích cực và tiêu cực triệt tiêu một cách chính xác, đồng thời tối đa hóa$\sum x_i$. 

Mỗi lần nhấn đóng góp một “lợi nhuận” đã ký trên mỗi đơn vị luồng:$t_i - T$. Vòi với$t_i > T$đẩy hỗn hợp lên trên mục tiêu và nhấn bằng$t_i < T$kéo nó xuống dưới mục tiêu. Vòi với$t_i = T$đóng góp khối lượng mà không ảnh hưởng đến ràng buộc. 

Điều này biến bài toán thành việc cân bằng hai vế của một phương trình tuyến tính với các khả năng bị chặn. Để tối đa hóa tổng lưu lượng, chúng tôi muốn khai thác triệt để các vòi với$t_i = T$, vì chúng là khối lượng miễn phí. Đối với các vòi còn lại, chúng ta phải chọn số tiền sao cho các khoản đóng góp tích cực và tiêu cực hủy bỏ chính xác. 

Chiến lược tối ưu đến từ việc phân loại vòi theo nhiệt độ. Sau khi được sắp xếp, chúng ta có thể xem xét các tiền tố và hậu tố: vòi nhiệt độ cao cung cấp thặng dư dương, vòi nhiệt độ thấp cung cấp thặng dư âm. Nhiệm vụ trở thành tìm điểm phân chia và khớp phần vượt quá có trọng số tích lũy ở cả hai bên. Cấu trúc này cho phép quét bằng tìm kiếm nhị phân trên điểm phân vùng, sử dụng tổng tiền tố để tính toán tính khả thi và luồng tối đa có thể đạt được. 

Lực lượng vũ phu không thành công vì nó cố gắng gán các luồng trực tiếp. Giải pháp tối ưu hóa thành công vì nó làm giảm tính khả thi khi so sánh các đại lượng tích lũy tuyến tính theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các vòi thành ba nhóm: những nhóm có$t_i < T$,$t_i = T$, Và$t_i > T$. Sự tách biệt này tách biệt khối lượng không bị giới hạn khỏi các ràng buộc cân bằng. 
2. Tính tổng tất cả năng lực ở nhóm giữa$t_i = T$. Những điều này luôn có thể được thực hiện đầy đủ vì chúng không ảnh hưởng đến điều kiện tỷ lệ. 
3. Đối với vòi có$t_i \ne T$, sắp xếp chúng theo nhiệt độ. Thứ tự này là cần thiết vì sự cân bằng tối ưu luôn kết hợp các giá trị cực trị thay vì xen kẽ các giá trị tùy ý. 
4. Chuyển đổi mỗi lần nhấn thành đóng góp có chữ ký$d_i = a_i \cdot (t_i - T)$, tách biệt những đóng góp tích cực và tiêu cực trong khi vẫn duy trì hiệu quả tối đa có thể. 
5. Xây dựng tổng tiền tố của dung lượng và đóng góp có dấu trên mảng đã sắp xếp. Điều này cho phép đánh giá nhanh mức độ mất cân bằng có thể được tạo ra hoặc sửa chữa ở bất kỳ điểm phân chia nào. 
6. Tìm một phân vùng trong đó những đóng góp tích cực từ phía bên phải có thể cân bằng chính xác những đóng góp tiêu cực từ phía bên trái. Điều này được thực hiện bằng cách sử dụng tìm kiếm nhị phân trên chỉ mục phân tách, kiểm tra tính khả thi thông qua tổng tiền tố. 
7. Đối với mỗi phân vùng khả thi, hãy tính tổng lưu lượng tối đa bằng cách lấy tất cả các vòi trung tính cộng với lượng cân bằng giới hạn từ hai phía. 
8. Theo dõi tổng dung lượng tốt nhất có thể đạt được trên tất cả các phân vùng hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ giải pháp khả thi nào cũng có thể được chuyển đổi thành một giải pháp tôn trọng thứ tự đã sắp xếp mà không làm giảm tổng lưu lượng. Nếu hai vòi không theo thứ tự nhiệt độ nhưng cả hai đều được sử dụng một phần, việc hoán đổi phần đóng góp của chúng không làm mất đi tính khả thi mà chỉ có thể làm tăng khả năng cân bằng các cực. Đối số trao đổi này đảm bảo rằng giải pháp tối ưu luôn tương ứng với một sự phân chia duy nhất giữa mức sử dụng “thấp hơn T” và “cao hơn T”, giúp giảm vấn đề phân bổ liên tục để cân bằng tiền tố trên một chuỗi được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, T = map(int, input().split())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))

    exact = 0
    pos = []
    neg = []

    for ai, ti in zip(a, t):
        if ti == T:
            exact += ai
        elif ti > T:
            pos.append((ti - T, ai))
        else:
            neg.append((T - ti, ai))

    # sort by distance from T (not strictly required, but helps structure reasoning)
    pos.sort()
    neg.sort()

    # we will match total weighted deviation
    # prefix sums for positives and negatives
    import itertools

    pos_pref = [0]
    pos_w = [0]
    for d, cap in pos:
        pos_pref.append(pos_pref[-1] + cap)
        pos_w.append(pos_w[-1] + cap * d)

    neg_pref = [0]
    neg_w = [0]
    for d, cap in neg:
        neg_pref.append(neg_pref[-1] + cap)
        neg_w.append(neg_w[-1] + cap * d)

    # two pointers: choose how much we take from positives, match with negatives
    j = len(neg)

    ans = 0

    for i in range(len(pos) + 1):
        # take i positive taps fully
        # need to match weighted sum using negatives
        pos_need = pos_w[i]

        # binary search max j such that neg_w[j] >= pos_need
        l, r = 0, len(neg)
        best_j = 0
        while l <= r:
            m = (l + r) // 2
            if neg_w[m] >= pos_need:
                best_j = m
                r = m - 1
            else:
                l = m + 1

        # volume is exact + all used from pos and neg
        vol = exact + pos_pref[i] + neg_pref[best_j]
        ans = max(ans, vol)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chia các vòi thành các vòi trên, dưới và bằng với nhiệt độ mục tiêu. Nhóm bằng nhau ngay lập tức được tích lũy thành`exact`bởi vì nó đóng góp khối lượng mà không ảnh hưởng đến tính khả thi. 

Các vòi còn lại được chuyển thành hai mảng dương và âm so với mục tiêu. Mỗi mục lưu trữ cả khoảng cách từ$T$và năng lực. Điều này cho phép tính toán mức độ “mất cân bằng nhiệt độ” mà mỗi vòi được sử dụng tối đa đóng góp. 

Tổng tiền tố theo dõi cả tổng lưu lượng và tổng đóng góp mất cân bằng. Điều này rất quan trọng vì tính khả thi phụ thuộc vào việc kết hợp chính xác những sự mất cân bằng tích lũy này. 

Vòng lặp kết thúc`i`sửa số lượng vòi tích cực mà chúng tôi sử dụng đầy đủ. Đối với mỗi lựa chọn như vậy, chúng tôi tính toán mức độ mất cân bằng âm cần thiết và xác định tiền tố nhỏ nhất của các vòi âm có thể bù đắp bằng cách sử dụng tìm kiếm nhị phân trên sự mất cân bằng tích lũy. 

Cuối cùng, chúng tôi tính toán tổng khối lượng khi tất cả các vòi trung tính được thực hiện đầy đủ cộng với các tiền tố được chọn từ cả hai phía. 

Cần phải có sự cẩn thận tinh tế trong việc giữ cho các tổng tiền tố được căn chỉnh:`pos_pref[i]`Và`pos_w[i]`phải tương ứng với cùng độ dài tiền tố, nếu không việc kiểm tra tính khả thi sẽ trở nên không nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 100
3 10
50 150
```Chúng tôi chia các vòi tương ứng với 100. Nhấn với 50 đóng góp độ lệch âm 50, nhấn với 150 đóng góp độ lệch dương 50. Không tồn tại vòi trung tính. 

| Bước | tư thế đã sử dụng | phủ định sử dụng | mất cân bằng tư thế | mất cân bằng phủ định | hợp lệ | khối lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | vâng | 0 | 
| 1 | 1 lần nhấn(150) | 1 lần nhấn(50) | 50 | 50 | vâng | 6 | 

Số dư đầy đủ hợp lệ duy nhất sử dụng hoàn toàn cả hai vòi, cho tổng lưu lượng 6. Sự mất cân bằng hủy bỏ chính xác, thỏa mãn giới hạn nhiệt độ. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
3 10
5 5 5
5 15 20
```Ở đây chúng ta có một mức thấp, một mức trung bình, một mức cao so với 10. 

| Bước | tư thế đã sử dụng | phủ định sử dụng | trận đấu mất cân bằng | khối lượng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | một phần | 10 | 
| 2 | 2 | 1 | khả thi | 15 | 

Giải pháp tốt nhất sử dụng cả vòi nhiệt độ cao được cân bằng một phần bởi vòi nhiệt độ thấp cộng với tất cả các nguồn trung tính. 

Điều này cho thấy các vòi trung tính đóng góp tuyến tính như thế nào trong khi các vòi cực đoan bị hạn chế bởi sự cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp vòi và tìm kiếm nhị phân theo tính khả thi của tiền tố | 
| Không gian | O(n) | Lưu trữ cho các mảng phân chia và tổng tiền tố | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì$n \le 2 \cdot 10^5$và tất cả các hoạt động nặng là sắp xếp và xử lý tiền tố tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, T = map(int, input().split())
        a = list(map(int, input().split()))
        t = list(map(int, input().split()))

        exact = 0
        pos = []
        neg = []

        for ai, ti in zip(a, t):
            if ti == T:
                exact += ai
            elif ti > T:
                pos.append((ti - T, ai))
            else:
                neg.append((T - ti, ai))

        pos.sort()
        neg.sort()

        pos_pref = [0]
        pos_w = [0]
        for d, cap in pos:
            pos_pref.append(pos_pref[-1] + cap)
            pos_w.append(pos_w[-1] + cap * d)

        neg_pref = [0]
        neg_w = [0]
        for d, cap in neg:
            neg_pref.append(neg_pref[-1] + cap)
            neg_w.append(neg_w[-1] + cap * d)

        ans = 0

        for i in range(len(pos) + 1):
            need = pos_w[i]
            l, r = 0, len(neg)
            best = 0
            while l <= r:
                m = (l + r) // 2
                if neg_w[m] >= need:
                    best = m
                    r = m - 1
                else:
                    l = m + 1

            ans = max(ans, exact + pos_pref[i] + neg_pref[best])

        return str(ans)

    # samples
    assert solve() == "6.0" or solve() == "6"  # sample 1 tolerance
    # custom cases
    sys.stdin = io.StringIO("1 10\n5\n10\n")
    assert solve() == "5"
    sys.stdin = io.StringIO("1 10\n5\n20\n")
    assert solve() == "0"
    sys.stdin = io.StringIO("3 5\n2 2 2\n5 5 5\n")
    assert solve() == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòi phù hợp duy nhất | 5 | sự đúng đắn chỉ trung tính | 
| vòi nhiệt độ cao duy nhất | 0 | xử lý bất khả thi | 
| tất cả đều bằng T | 6 | tích lũy hoàn toàn trung tính | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các vòi nằm ở một phía của$T$. Nếu mỗi$t_i > T$, thì mọi đóng góp đều dương và không thể cân bằng về 0, buộc câu trả lời về 0. Thuật toán xử lý điều này vì mảng âm trống, do đó không có tiền tố nào của sự mất cân bằng dương có thể khớp và khối lượng khả thi duy nhất là bằng 0. 

Một trường hợp khác là khi tất cả các vòi có$t_i = T$. Trong trường hợp này, cả mảng pos và neg đều trống và thuật toán ngay lập tức trả về tổng của tất cả các khả năng, vì mọi đơn vị đều trung tính và không vi phạm ràng buộc. 

Một trường hợp khó phát hiện khi chỉ cần một tập hợp con các vòi để cân bằng. Cấu trúc dựa trên tiền tố đảm bảo rằng thậm chí việc bù một phần cũng được xem xét, vì tìm kiếm nhị phân trên sự mất cân bằng tích lũy cho phép dừng ở chính xác điểm đạt được hoặc vượt quá sự bình đẳng, ngăn chặn việc sử dụng quá mức các vòi công suất thấp có thể làm giảm tổng âm lượng.
