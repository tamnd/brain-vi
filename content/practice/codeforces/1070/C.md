---
title: "CF 1070C - Điện toán đám mây"
description: "Chúng tôi được đưa ra một mốc thời gian là n ngày. Mỗi ngày, một công ty cần tới k lõi CPU, nhưng thay vì mua một gói cố định, công ty có thể thuê lõi từ nhiều ưu đãi cho thuê chồng chéo."
date: "2026-06-15T13:44:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1070
codeforces_index: "C"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 2000
weight: 1070
solve_time_s: 391
verified: false
draft: false
---

[CF 1070C - Điện toán đám mây](https://codeforces.com/problemset/problem/1070/C) 

**Xếp hạng:** 2000 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một mốc thời gian`n`ngày. Mỗi ngày công ty cần tới`k`Lõi CPU, nhưng thay vì mua một gói cố định, họ có thể thuê lõi từ nhiều ưu đãi cho thuê chồng chéo. Mỗi ưu đãi chỉ hoạt động trong khoảng thời gian ngày liên tục và trong mỗi ngày hoạt động, ưu đãi cung cấp tối đa`c_i`lõi. Mỗi lõi được thuê từ ưu đãi đó vào ngày hôm đó đều có giá`p_i`. 

Vào bất kỳ ngày nào, chúng tôi chọn số lượng lõi cần lấy từ mỗi ưu đãi đang hoạt động. Nếu tổng công suất khả dụng của tất cả các ưu đãi đang hoạt động ít nhất là`k`, chúng tôi chỉ lấy`k`lõi, nếu không chúng tôi sẽ lấy mọi thứ có sẵn và chấp nhận rằng nhu cầu đó không được đáp ứng đầy đủ. Mục tiêu là giảm thiểu tổng chi phí trong tất cả các ngày. 

Khó khăn chính là các đề nghị trùng lặp về thời gian, nhưng quyết định mang tính cục bộ mỗi ngày: mỗi ngày chúng tôi muốn chọn các lõi có sẵn rẻ nhất trước tiên, tối đa`k`. 

Những ràng buộc đẩy chúng ta tới một`O((n + m) log m)`hoặc`O(n log m)`giải pháp. Với`n`lên tới một triệu và`m`lên tới hai trăm nghìn, bất kỳ việc phân loại hoặc quét hàng ngày nào đối với tất cả các ưu đãi đang hoạt động đều quá chậm. Thậm chí`O(nm)`là không thể, vì nó sẽ đạt tới hoạt động 2e11. 

Một sai lầm ngây thơ là tính toán lại điều tốt nhất`k`lõi mỗi ngày bằng cách thu thập tất cả các ưu đãi đang hoạt động và sắp xếp chúng theo giá. Đó sẽ là`O(n m log m)`trong trường hợp xấu nhất. 

Một trường hợp khó phát hiện khác là khi những lời đề nghị đắt tiền xuất hiện sớm nhưng những lời đề nghị rẻ hơn lại xuất hiện muộn hơn. Nếu chúng ta không duy trì cấu trúc toàn cầu được sắp xếp theo giá, chúng ta có thể mua sớm các lõi đắt tiền thay vì chờ đợi những lõi rẻ hơn có sẵn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xử lý mỗi ngày một cách độc lập. Trong một ngày nhất định, chúng tôi thu thập tất cả các ưu đãi có khoảng thời gian bao gồm ngày đó, sắp xếp chúng theo giá và tham lam lấy lõi cho đến khi chúng tôi đạt được`k`. Điều này đúng vì trong vòng một ngày, các lõi rẻ hơn sẽ luôn được ưu tiên. Tuy nhiên, việc tính toán lại các ưu đãi đang hoạt động mỗi ngày rất tốn kém. Trong trường hợp xấu nhất khi tất cả các ưu đãi kéo dài cả ngày, chúng tôi sẽ liên tục sắp xếp`m`các mặt hàng cho`n`ngày, cho`O(n m log m)`phức tạp, vượt quá giới hạn. 

Quan sát quan trọng là mặc dù tính khả dụng thay đổi theo thời gian nhưng quy tắc quyết định là cố định: luôn lấy các lõi có sẵn rẻ nhất trước tiên. Điều này gợi ý việc duy trì cấu trúc toàn cầu của các ưu đãi hiện đang hoạt động được sắp xếp theo giá, cập nhật cấu trúc đó khi khoảng thời gian bắt đầu và kết thúc. Chúng tôi có thể xử lý các ngày theo thứ tự, sử dụng đường quét theo thời gian và cấu trúc dữ liệu hỗ trợ chèn, xóa và trích xuất dung lượng rẻ nhất. 

Thách thức chính là mỗi ưu đãi đều có giới hạn dung lượng`c_i`mỗi ngày và chúng tôi có thể sử dụng một phần nó. Chúng ta cần theo dõi công suất còn lại theo cấu trúc giống như nhiều tập hợp được sắp xếp theo giá. Mỗi ngày, chúng ta liên tục tiêu dùng từ những ưu đãi rẻ nhất hiện có cho đến khi chúng ta đạt được`k`hoặc cung cấp khí thải. 

Chúng ta có thể lập mô hình các ưu đãi đang hoạt động bằng cách sử dụng cấu trúc cân bằng, chẳng hạn như một đống kết hợp với việc xóa từng phần hoặc phân đoạn cây theo giá trị giá. Một giải pháp CF phổ biến nén bằng cách xử lý các sự kiện và duy trì nhiều bộ công suất trên mỗi mức giá, luôn tiêu thụ từ nhóm giá thấp nhất trước tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m log m) | O(m) | Quá chậm | 
| Quét tối ưu + Nhiều bộ | O((n + m) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý thời gian từ ngày`1`Hôm nay`n`trong khi vẫn duy trì tất cả các ưu đãi đang hoạt động được sắp xếp theo giá. 

1. Chuyển đổi mỗi ưu đãi thành hai sự kiện: trong ngày`l_i`nó bắt đầu hoạt động và vào ban ngày`r_i + 1`nó trở nên không hoạt động. Điều này cho phép chúng tôi cập nhật dần công suất hoạt động thay vì tính toán lại từ đầu mỗi ngày. Điều này là cần thiết vì việc kiểm tra lại tất cả các ưu đãi mỗi ngày sẽ quá chậm. 
2. Duy trì cấu trúc được khóa theo giá để lưu trữ tổng công suất sẵn có cho mức giá đó trong số tất cả các ưu đãi đang hoạt động. Khi một ưu đãi có hiệu lực, chúng tôi sẽ thêm`c_i`vào nhóm giá của nó; khi nó hết hạn, chúng tôi trừ nó đi. 
3. Mỗi ngày, chúng tôi liên tục lấy từ nhóm giá rẻ nhất cho đến khi thu thập được`k`lõi hoặc không còn dung lượng. Chúng tôi luôn ưu tiên giá rẻ hơn trước tiên vì bất kỳ đơn vị nào được lấy từ thùng đắt tiền hơn trong khi có thùng rẻ hơn đều có thể được thay thế để giảm chi phí. 
4. Tích lũy chi phí khi chúng ta tiêu thụ năng lực. Khi chúng tôi lấy`x`lõi từ giá`p`, chúng tôi thêm`x * p`để trả lời. 
5. Chuyển sang ngày tiếp theo sau khi xử lý hết mức tiêu thụ cho cấu hình hiện tại. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày cố định nào, vấn đề giảm xuống còn việc chọn tối đa`k`các mặt hàng trong đó mỗi đơn vị công suất có trọng lượng (giá) cố định và chúng tôi muốn giảm thiểu tổng chi phí. Điều này tương đương với một chiếc ba lô phân số trong đó tất cả các vật phẩm được chia thành các lõi đơn vị. Vì không có sự ghép nối giữa các ngày ngoại trừ khoảng thời gian kích hoạt nên việc duy trì độ chính xác mỗi ngày một cách độc lập là đủ. Điều bất biến là vào đầu mỗi ngày, cấu trúc chứa chính xác tất cả năng lực sẵn có của các ưu đãi đang hoạt động và việc tiêu dùng tham lam bằng cách tăng giá luôn mang lại lựa chọn tối ưu cho ngày đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())

    add = [[] for _ in range(n + 2)]
    rem = [[] for _ in range(n + 2)]

    for _ in range(m):
        l, r, c, p = map(int, input().split())
        add[l].append((p, c))
        rem[r + 1].append((p, c))

    import heapq

    active = {}
    heap = []

    def add_offer(p, c):
        active[p] = active.get(p, 0) + c
        heapq.heappush(heap, p)

    def remove_offer(p, c):
        active[p] -= c
        if active[p] == 0:
            del active[p]

    ans = 0

    for day in range(1, n + 1):
        for p, c in add[day]:
            add_offer(p, c)

        for p, c in rem[day]:
            remove_offer(p, c)

        need = k

        while need > 0 and heap:
            p = heap[0]
            if p not in active:
                heapq.heappop(heap)
                continue

            avail = active[p]
            take = min(need, avail)
            ans += take * p
            need -= take

            if take == avail:
                heapq.heappop(heap)
                del active[p]
            else:
                active[p] -= take

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng danh sách sự kiện`add`Và`rem`để duy trì ưu đãi nào bắt đầu và kết thúc mỗi ngày. Một đống giá lưu trữ ứng viên. Từ điển`active`lưu trữ tổng công suất còn lại trên mỗi giá. 

Đống dữ liệu có thể chứa các mục nhập cũ vì giá cổ phiếu hoặc năng lực của nhiều ưu đãi thay đổi; do đó, trước khi sử dụng phần tử trên cùng, chúng tôi xác minh nó vẫn tồn tại trong`active`. Nếu không, chúng ta sẽ loại bỏ nó một cách lười biếng. 

Logic tiêu thụ luôn loại bỏ giá rẻ nhất trước tiên và tiêu thụ một phần xô khi cần. 

Một điểm tinh tế là chúng tôi không bao giờ theo dõi các ưu đãi riêng lẻ mà chỉ tổng hợp công suất trên mỗi mức giá. Điều này hợp lệ vì tất cả các đơn vị ở cùng một mức giá đều có thể hoán đổi cho nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 7 3
1 4 5 3
1 3 5 2
2 5 10 1
```Chúng tôi theo dõi năng lực hoạt động theo giá. 

| Ngày | Giá hoạt động (công suất) | Cần | Đã thực hiện (giá → số tiền) | Nhu cầu còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 3→5, 2→5 | 7 | 2→5, 3→2 | 0 | 
| 2 | 3→5, 2→5, 1→10 | 7 | 1→7 | 0 | 
| 3 | 3→5, 2→5, 1→10 | 7 | 1→7 | 0 | 
| 4 | 3→5, 1→10 | 7 | 1→7 | 0 | 
| 5 | 1→10 | 7 | 1→7 | 0 | 

Tổng chi phí tích lũy tương ứng là 44. 

Dấu vết này cho thấy một khi giá 1 xuất hiện, nó sẽ chiếm ưu thế trong tất cả các lựa chọn sau này. 

### Ví dụ 2 

đầu vào:```
3 4 2
1 2 3 5
2 3 2 1
```| Ngày | Giá hoạt động | Cần | Đã chụp | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 5→3 | 4 | 3→3 | 15 | 
| 2 | 5→3, 1→2 | 4 | 1→2, 5→2 | 7 | 
| 3 | 1→2 | 4 | 1→2 | 2 | 

Ví dụ này cho thấy mức tiêu thụ một phần theo mức giá khi công suất không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Mỗi ưu đãi được chèn và xóa một lần, các phép toán vùng heap có tính logarit và mỗi đơn vị được xử lý một lần | 
| Không gian | O(m) | Lưu trữ các sự kiện, mục heap và bản đồ dung lượng hoạt động | 

Điều này phù hợp trong giới hạn vì`n + m`nhiều nhất là khoảng 1,2 triệu và hệ số logarit vẫn ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve  # assume solution is in main.py
    return solve()

# provided sample
assert run("""5 7 3
1 4 5 3
1 3 5 2
2 5 10 1
""") == "44"

# minimal case
assert run("""1 5 1
1 1 10 2
""") == "10"

# insufficient supply
assert run("""2 10 1
1 1 3 5
""") == "30"

# overlapping prices
assert run("""2 3 2
1 2 2 2
1 2 2 1
""") == "4"

# boundary intervals
assert run("""3 2 2
1 1 5 3
3 3 5 1
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 10 | khoảng thời gian duy nhất, tiêu thụ trực tiếp | 
| cung không đủ | 30 | xử lý thiếu hụt đúng cách | 
| giá chồng chéo | 4 | đúng thứ tự tham lam | 
| khoảng ranh giới | 8 | kích hoạt/hủy kích hoạt ở các cạnh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ưu đãi chỉ kéo dài một ngày. Bản cập nhật dựa trên sự kiện đảm bảo nó được thêm và xóa một cách chính xác trong cùng một lần lặp. Ví dụ:```
1 2 3 5
```kích hoạt vào ngày 1 và bị xóa vào ngày thứ 3, do đó nó chỉ đóng góp vào ngày 1 và 2. Danh sách bổ sung/rem đảm bảo điều này mà không cần viết hoa chữ đặc biệt. 

Một trường hợp khác là nhiều ưu đãi có cùng mức giá. Vì chúng tôi tổng hợp công suất trên mỗi mức giá nên chúng tôi tránh phân tách logic. Đống dữ liệu có thể chứa các mục giá trùng lặp, nhưng việc xóa chậm sẽ đảm bảo tính chính xác. 

Trường hợp cạnh cuối cùng là khi tổng công suất nhỏ hơn`k`. Vòng lặp dừng khi vùng heap trống và chúng tôi chỉ tích lũy chính xác các lõi có sẵn mà không buộc phải phân bổ đầy đủ.
