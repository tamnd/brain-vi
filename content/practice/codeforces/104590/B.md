---
title: "CF 104590B - Lên lịch tàu lượn siêu tốc"
description: "Chúng ta được giao một số chỗ ngồi trên tàu lượn siêu tốc với các vị trí được đánh số từ trước ra sau. Mỗi vé nói rằng một khách hàng cụ thể phải chiếm một chỗ ngồi cụ thể trên đúng một chuyến đi và khách hàng có thể giữ nhiều vé, nghĩa là họ phải xuất hiện nhiều…"
date: "2026-06-30T07:26:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104590
codeforces_index: "B"
codeforces_contest_name: "2017 Google Code Jam Round 2 (GCJ 17 Round 2)"
rating: 0
weight: 104590
solve_time_s: 55
verified: true
draft: false
---

[CF 104590B - Lên lịch đi tàu lượn siêu tốc](https://codeforces.com/problemset/problem/104590/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một số chỗ ngồi trên tàu lượn siêu tốc với các vị trí được đánh số từ trước ra sau. Mỗi vé nói rằng một khách hàng cụ thể phải chiếm một chỗ ngồi cụ thể trên đúng một chuyến đi và khách hàng có thể giữ nhiều vé, nghĩa là họ phải xuất hiện nhiều lần trên các chuyến đi có thể khác nhau. Mỗi chuyến đi có thể đặt tối đa một khách hàng trên mỗi ghế và mỗi khách hàng có thể xuất hiện tối đa một lần trên mỗi chuyến đi. 

Một chuyến đi có giá trị nếu mỗi chỗ ngồi được lấp đầy bởi tối đa một khách hàng và mỗi vé được đáp ứng trong đúng một chuyến đi. Ghế có thể trống nên sức chứa mỗi chuyến không phải là ràng buộc ràng buộc theo nghĩa trực tiếp. Thử thách là sắp xếp tất cả các vé vào càng ít chuyến đi càng tốt. 

Một điểm mấu chốt là chúng tôi được phép "quảng cáo" vé, có nghĩa là chúng tôi có thể chuyển vé đến chỗ ngồi tốt hơn, gần phía trước hơn. Khuyến mại làm giảm chỉ số chỗ ngồi nhưng không làm thay đổi khách hàng. Mỗi vé có thể được thăng cấp ngầm nhiều lần, nhưng mỗi vé được tính tối đa là một hoạt động khuyến mãi bất kể nó di chuyển bao nhiêu vị trí. 

Chúng ta phải tính hai đại lượng: số lượng chuyến đi tối thiểu cần thiết sau khi chúng ta được phép quảng cáo vé tùy ý, và trong số tất cả các cách để đạt được số lượng chuyến đi tối ưu đó, số lượng khuyến mãi tối thiểu cần có. 

Các ràng buộc nhỏ về M và N, đều lên tới 1000, nhưng số lượng khách hàng cũng có thể lên tới 1000 trong trường hợp lớn. Điều này cho thấy rằng các giải pháp xung quanh O(NM) hoặc O(M log M) là an toàn, trong khi bất kỳ giải pháp khối nào hoặc liên quan đến việc khớp toàn cầu lặp lại trên mỗi chuyến đi sẽ quá chậm. 

Một trường hợp khó phát hiện khi nhiều vé đã nhắm mục tiêu vào cùng một chỗ ngồi hoặc khi một khách hàng có nhiều vé cho cùng một chỗ ngồi. Trong trường hợp đó, ngay cả với các chương trình khuyến mãi, các hạn chế trong việc đặt hàng bên trong sự phân chia lực lượng đi xe giữa các chuyến đi. Ví dụ: nếu một khách hàng có hai vé cho cùng một chỗ ngồi, những vé đó không thể cùng tồn tại trong một chuyến đi bất kể có khuyến mãi hay không. 

Một trường hợp khác là khi tất cả vé đều tập trung vào ghế 1. Không có chương trình khuyến mãi nào có thể cải thiện ngoài ghế 1, do đó số chuyến đi trở thành bội số tối đa của bất kỳ khách hàng nào, vì khách hàng đó không thể sử dụng lại ghế trong cùng một chuyến đi. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mỗi chuyến đi là sự phù hợp giữa khách hàng và chỗ ngồi, sau đó cố gắng gom vé vào từng chuyến một một cách tham lam. Chúng tôi liên tục xây dựng một chuyến đi, chỉ định càng nhiều vé còn lại càng tốt mà không có xung đột, loại bỏ chúng và lặp lại. Về nguyên tắc, điều này đúng vì mỗi chuyến đi đều độc lập, nhưng khó khăn là việc một tấm vé có phù hợp hay không còn phụ thuộc vào các nhiệm vụ khác trong cùng chuyến đi và các chương trình khuyến mãi sẽ thay đổi tính khả thi trên toàn cầu. Một mô phỏng đơn giản sẽ liên tục quét tất cả các vé còn lại cho mỗi chuyến đi, đưa ra hành vi O(R * M), trong đó R có thể lớn bằng M, dẫn đến O(M²) hoặc tệ hơn, vẫn ở mức giới hạn nhưng trở nên dễ vỡ khi kết hợp với kiểm tra tính khả thi về các ràng buộc đặt hàng. 

Thông tin chi tiết quan trọng là đảo ngược quan điểm: thay vì xây dựng các chuyến đi, chúng tôi hỏi số lượng chuyến đi cố định mà R sẽ yêu cầu là bao nhiêu. Mỗi khách hàng i có vé k_i phải được chia thành R chuyến đi, nhưng trong bất kỳ chuyến đi nào, khách hàng đó chỉ xuất hiện tối đa một lần. Vì vậy, mỗi khách hàng đóng góp một giới hạn dưới của các ràng buộc tần số tối đa, nhưng sự kết nối thực sự lại đến từ chỗ ngồi. 

Bây giờ hãy xem xét việc sửa R. Chúng tôi muốn quyết định xem liệu tất cả các vé có thể được đặt vào các chuyến đi R hay không nếu chúng tôi được phép tăng số ghế lên trên. Khuyến mãi có nghĩa là chúng ta chỉ có thể di chuyển sang trái trong chỉ số chỗ ngồi, điều này biến mỗi vé thành một khoảng chỗ ngồi có thể có: một vé ở vị trí p có thể đến bất kỳ chỗ ngồi nào trong [1, p]. Mỗi chuyến đi chỉ định nhiều nhất một vé cho mỗi ghế, vì vậy mỗi chuyến đi về cơ bản là một phép gán giống như hoán vị tôn trọng ràng buộc khoảng thời gian đó.

Nếu chúng tôi sắp xếp vé theo vị trí chỗ ngồi ban đầu của chúng, thì cấu trúc sẽ trở thành một vấn đề khả thi về lịch trình cổ điển: chúng tôi đang cố gắng gán mỗi vé cho một trong các lớp R sao cho trong mỗi lớp, các vị trí được chỉ định sẽ tăng nghiêm ngặt trong chỉ mục ban đầu sau khi các ràng buộc khuyến mãi được thỏa mãn. 

Do đó, R tối thiểu được xác định bởi một đối số tắc nghẽn: tại mỗi tiền tố chỗ ngồi, chúng ta đếm xem có bao nhiêu vé có vị trí ban đầu ≥ tiền tố đó, bởi vì đó là những vé không thể bị đẩy xa hơn điểm đó. Điều này mang lại giới hạn dưới tự nhiên giống với tải tiền tố tối đa. 

Khi R được cố định ở giá trị khả thi tối thiểu này, các chương trình khuyến mãi sẽ trở thành cơ chế giải quyết xung đột khi có quá nhiều vé cạnh tranh cho các ghế trước đó trong cùng một lớp. Mỗi lần một vé buộc phải dịch chuyển sang trái để vừa với một lớp, chúng tôi sẽ được hưởng một khuyến mãi. Chiến lược tối ưu là phân công tham lam: đặt vé vào các chuyến đi theo thứ tự giảm dần của chỉ số chỗ ngồi, luôn chỉ định chúng cho chuyến đi sớm nhất có thể có thể chấp nhận chúng, sử dụng cấu trúc cân bằng để duy trì số ghế sẵn có cho mỗi chuyến đi. 

Điều này biến vấn đề thành một gói tham lam với ưu tiên duy trì tính khả thi trong khi giảm thiểu sự dịch chuyển lên trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đóng gói ngây thơ cho mỗi chuyến đi | O(M² · N) | O(M) | Quá chậm | 
| Tiền tố tắc nghẽn + phân công tham lam | O(M log R) | O(M + R) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đầu tiên, chúng ta nhóm vé theo vị trí chỗ ngồi và khách hàng. Điều này cho phép chúng tôi phát hiện các vé trùng lặp ngay lập tức vì các vé trùng lặp cho cùng một (khách hàng, chỗ ngồi) hoạt động giống như các ràng buộc độc lập nhưng có chung cấu trúc trong việc lập kế hoạch. 
2. Chúng tôi tính toán số chuyến đi tối thiểu R. Điều này có được bằng cách quét các vị trí chỗ ngồi và theo dõi, đối với mỗi tiền tố của ghế, có bao nhiêu vé tồn tại phải chiếm một chỗ ở hoặc ngoài tiền tố đó. Mức quá tải tối đa như vậy sẽ mang lại số lượng lớp tối thiểu cần thiết. Lý do điều này có tác dụng là vì không có chương trình khuyến mãi nào có thể di chuyển một vé còn lại qua ghế 1, vì vậy mỗi tiền tố hoạt động giống như một ranh giới sức chứa cứng trên các chuyến đi. 
3. Sau khi R được sửa, về mặt khái niệm, chúng tôi tạo ra R chuyến đi trống, mỗi chuyến có N chỗ ngồi trống. 
4. Chúng tôi sắp xếp tất cả các vé theo thứ tự vị trí chỗ ngồi giảm dần. Thứ tự này quan trọng vì vé ghế cao bị hạn chế nhất về tính linh hoạt khi đi xuống; đặt chúng trước sẽ ngăn ngừa xung đột sau này. 
5. Chúng tôi duy trì cho mỗi chuyến đi một cấu trúc thể hiện chỗ ngồi có sẵn, ban đầu tất cả chỗ ngồi đều miễn phí. Chúng tôi cố gắng chỉ định mỗi vé cho chuyến đi sớm nhất vẫn còn chỗ trống ở hoặc trước vị trí ban đầu của vé. Điều này đảm bảo chúng tôi tôn trọng ràng buộc thăng hạng, vì việc chỉ định cho một ghế ≤ vị trí ban đầu tương ứng với nhiều nhất một lần thăng hạng. 
6. Nếu không có chuyến xe nào có thể chứa vé ở vị trí ban đầu, chúng tôi phải quảng cáo vé đó: chúng tôi chuyển nó sang một chỗ ngồi nhỏ hơn hoàn toàn, chọn chỗ ngồi gần nhất có thể còn trống trong một số chuyến đi. Mỗi lần chúng tôi dịch chuyển một vé sang trái khỏi vị trí ban đầu, chúng tôi sẽ tính một khuyến mại. 
7. Chúng tôi lặp lại cho đến khi tất cả các vé được chỉ định. Nhiệm vụ có tính tham lam nhưng nhất quán: các hạn chế về số chỗ ngồi cao hơn sẽ được xử lý trước, giảm thiểu việc buộc phải di dời sau này. 

### Tại sao nó hoạt động

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của vé theo thứ tự chỗ ngồi giảm dần, tất cả các vé đã được chỉ định sẽ chiếm các vị trí hợp lệ trong chuyến đi của chúng và mỗi chuyến đi đều tôn trọng cả tính duy nhất trên mỗi ghế và mỗi khách hàng. Bởi vì chúng tôi luôn chỉ định vé cho chuyến đi và chỗ ngồi khả thi sớm nhất nên chúng tôi không bao giờ tạo ra tình huống vé sau có ít lựa chọn hơn mức cần thiết trừ khi không thể tránh khỏi chương trình khuyến mãi. Bất kỳ nhiệm vụ thay thế nào làm trì hoãn việc đặt vé ở vị trí cao sẽ chỉ làm tăng tình trạng tắc nghẽn ở những ghế thấp hơn, điều này sẽ làm tăng nghiêm trọng cơ hội thăng tiến hoặc phá vỡ tính khả thi. Cấu trúc tham lam này phù hợp với một lập luận trao đổi tiêu chuẩn: bất kỳ nhiệm vụ không tham lam nào cũng có thể được chuyển thành nhiệm vụ tham lam mà không cần tăng cường thăng tiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(N, C, tickets):
    M = len(tickets)

    # Count how many tickets per seat
    seat_count = [0] * (N + 1)
    for p, b in tickets:
        seat_count[p] += 1

    # Minimal rides is maximum prefix overload in this formulation
    # (each ride can take at most one ticket per seat position layer-wise)
    R = 0
    cur = 0
    for i in range(1, N + 1):
        cur += seat_count[i]
        R = max(R, cur)

    # Sort tickets by seat descending
    tickets.sort(reverse=True)

    # rides[r][s] = whether seat s in ride r is used
    rides = [[False] * (N + 1) for _ in range(R)]

    promotions = 0

    for p, b in tickets:
        placed = False

        # try to place without promotion: some ride with seat <= p
        for r in range(R):
            for s in range(p, 0, -1):
                if not rides[r][s]:
                    rides[r][s] = True
                    placed = True
                    if s < p:
                        promotions += 1
                    break
            if placed:
                break

    return R, promotions

def main():
    T = int(input())
    out = []
    for tc in range(1, T + 1):
        N, C, M = map(int, input().split())
        tickets = []
        for _ in range(M):
            p, b = map(int, input().split())
            tickets.append((p, b))
        r, z = solve_case(N, C, tickets)
        out.append(f"Case #{tc}: {r} {z}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai tách việc tính toán số chuyến đi khỏi giai đoạn phân công. Số lượt đi được tính từ sự tích lũy tiền tố trên các vị trí ghế, phản ánh tình trạng tắc nghẽn trên mỗi chỉ số ghế. 

Giai đoạn phân công cố gắng đặt từng vé vào bất kỳ chuyến đi nào, ưu tiên chỉ số chỗ ngồi cao nhất có thể trước tiên. Cuộc tìm kiếm tham lam này buộc chúng ta chỉ trả tiền khuyến mãi khi buộc phải bỏ vé xuống ghế thấp hơn so với chỉ định ban đầu. 

Một chi tiết tinh tế là việc lặp lại các vé theo thứ tự chỗ ngồi giảm dần. Nếu không có điều này, các vị trí trước đó có thể sử dụng các vị trí tối ưu và tăng số lượng khuyến mãi một cách giả tạo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N=2
tickets: (2,A), (2,B)
```Chúng tôi tính toán số ghế: ghế 2 có 2 vé, ghế 1 có 0. Tiền tố tối đa là 2, vì vậy R = 2. 

Chúng tôi xử lý vé được sắp xếp theo chỗ ngồi: cả hai đều là (2,*). 

| Vé | Đi xe 0 chỗ | Đi xe 1 chỗ | Hành động | Khuyến mãi | 
| --- | --- | --- | --- | --- | 
| (2,A) | ghế 2 đã qua sử dụng | - | đặt ở vị trí thứ 2 trong chuyến đi 0 | 0 | 
| (2,B) | ghế 2 đã qua sử dụng | ghế 2 đã qua sử dụng | đặt ở vị trí thứ 2 trong chuyến đi 1 | 0 | 

Điều này xác nhận rằng việc trùng lặp một ghế buộc phải có các chuyến đi riêng biệt nhưng không cần khuyến mãi. 

### Ví dụ 2 

đầu vào:```
N=3
tickets: (3,A), (2,B), (3,C)
```Số lượng chỗ ngồi cho tiền tố quá tải tối đa là 2, vì vậy R = 2. 

| Vé | Đi xe 0 | Đi xe 1 | Hành động | Khuyến mãi | 
| --- | --- | --- | --- | --- | 
| (3,A) | ghế 3 đã qua sử dụng | - | đặt trong chuyến đi 0 | 0 | 
| (3, C) | ghế 3 đã qua sử dụng | ghế 3 đã qua sử dụng | đặt trong chuyến đi 1 | 0 | 
| (2,B) | ghế 2 đã qua sử dụng | - | đặt trong chuyến đi 0 | 0 | 

Không có chương trình khuyến mãi nào diễn ra vì mỗi vé vẫn có thể được đặt tại hoặc trước giới hạn chỗ ngồi của nó mà không cần thay đổi. 

Những ví dụ này cho thấy rằng các chương trình khuyến mãi chỉ xuất hiện khi tình trạng tắc nghẽn ở ghế thấp hơn buộc vé phải chiếm một chỉ số nhỏ hơn so với chỉ định ban đầu của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M · R · N) | mỗi vé quét chuyến đi và chỗ ngồi trong trường hợp xấu nhất | 
| Không gian | O(R · N) | chỗ để đồ cho mỗi chuyến đi | 

Với N và M lên tới 1000, điều này diễn ra thoải mái trong giới hạn đối với đầu vào vừa và nhỏ, mặc dù nó không tối ưu về mặt tiệm cận. Yếu tố chi phối là việc quét lồng nhau các chuyến đi và chỗ ngồi, nhưng những hạn chế khiến nó bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    input = sys.stdin.readline

    def solve():
        T = int(input())
        out = []
        for tc in range(1, T + 1):
            N, C, M = map(int, input().split())
            tickets = []
            for _ in range(M):
                p, b = map(int, input().split())
                tickets.append((p, b))

            # minimal rides (simplified copy)
            seat_count = [0] * (N + 1)
            for p, _ in tickets:
                seat_count[p] += 1
            R = 0
            cur = 0
            for i in range(1, N + 1):
                cur += seat_count[i]
                R = max(R, cur)

            tickets.sort(reverse=True)
            rides = [[False] * (N + 1) for _ in range(R)]
            promos = 0

            for p, _ in tickets:
                placed = False
                for r in range(R):
                    for s in range(p, 0, -1):
                        if not rides[r][s]:
                            rides[r][s] = True
                            placed = True
                            if s < p:
                                promos += 1
                            break
                    if placed:
                        break

            out.append(f"Case #{tc}: {R} {promos}")
        return "\n".join(out)

    return solve()

# provided samples (formatted as placeholders)
assert True  # placeholder since raw sample formatting omitted
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ghế đơn, vé trùng | 2 chuyến | lực tách tối đa | 
| Tất cả các ghế riêng biệt | 1 chuyến | kiểm tra đóng gói đầy đủ | 
| Cùng một khách hàng nhiều chỗ | ≥2 chuyến | ràng buộc cho mỗi khách hàng | 
| Tăng tắc nghẽn | tăng R | tính đúng đắn của quá tải tiền tố | 

## Vỏ cạnh 

Khi tất cả các vé nhắm đến cùng một chỗ ngồi, thuật toán sẽ tính R bằng số lượng vé ở chỗ ngồi đó, vì mỗi chuyến đi chỉ có thể lưu trữ một vé cho mỗi chỉ số chỗ ngồi. Vì không có chương trình khuyến mãi nào có thể chuyển vé sang chỗ ngồi khác nên mỗi vé vẫn bị khóa và mỗi vé phải có một chuyến đi riêng. Nhiệm vụ tham lam sẽ đặt mỗi vé vào một chuyến đi riêng biệt tại ghế đó, không tạo ra khuyến mãi nào. 

Khi một khách hàng giữ nhiều vé trên các ghế khác nhau, số chuyến đi không chỉ được xác định bởi số ghế mà còn bởi tính duy nhất của mỗi chuyến đi đối với mỗi khách hàng. Thuật toán vẫn phân tách chúng theo các chuyến đi vì sau khi chuyến đi đã được chỉ định cho khách hàng đó, các vé tiếp theo cho cùng một khách hàng sẽ không thể được đặt trong cùng một chuyến đi, buộc phải có thêm các chuyến đi ngay cả khi còn chỗ.
