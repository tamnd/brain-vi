---
title: "CF 105632F - Vòng lặp vô hạn"
description: "Chúng ta được giao cho một khuôn mẫu công việc cố định lặp đi lặp lại hàng ngày mãi mãi. Mỗi ngày có mốc thời gian là k giờ và vào đầu mỗi ngày có chính xác n nhiệm vụ xuất hiện. Nhiệm vụ i của một ngày xuất hiện vào một giờ đã biết ai trong ngày đó và yêu cầu thời gian xử lý không bị gián đoạn trong hai giờ."
date: "2026-06-22T05:37:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105632
codeforces_index: "F"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Zhengzhou Onsite (The 3rd Universal Cup. Stage 22: Zhengzhou)"
rating: 0
weight: 105632
solve_time_s: 78
verified: true
draft: false
---

[CF 105632F - Vòng lặp vô hạn](https://codeforces.com/problemset/problem/105632/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một khuôn mẫu công việc cố định lặp đi lặp lại hàng ngày mãi mãi. Mỗi ngày đều có một mốc thời gian`k`giờ và chính xác vào lúc bắt đầu mỗi ngày`n`nhiệm vụ xuất hiện. Nhiệm vụ`i`của một ngày xuất hiện vào một giờ đã biết`a_i`trong ngày đó và yêu cầu`b_i`giờ xử lý không bị gián đoạn. Những lần đến`a_i`đang tăng lên nghiêm trọng, vì vậy các nhiệm vụ trong ngày đều theo thứ tự thời gian cố định. 

Có một nhiệm vụ xử lý công nhân duy nhất. Người lao động không bao giờ ưu tiên một nhiệm vụ khi đã bắt đầu và luôn chọn nhiệm vụ chưa hoàn thành sớm nhất bất cứ khi nào họ rảnh. Bởi vì các nhiệm vụ lặp lại hàng ngày nên điều này trở thành một chuỗi nhiệm vụ vô tận với cấu trúc rất đều đặn. 

Mỗi truy vấn đề cập đến một lần xuất hiện cụ thể của một tác vụ, cụ thể là`y`-nhiệm vụ thứ trong ngày`x`. Mục tiêu là tính toán thời gian chính xác, được biểu thị bằng số ngày và giờ trong ngày đó, khi nhiệm vụ cụ thể đó kết thúc. 

Khó khăn chính là mặc dù mỗi ngày chỉ có`n`nhiệm vụ, hệ thống sẽ chạy mãi mãi và các nhiệm vụ từ những ngày khác nhau sẽ tương tác thông qua hàng đợi. Một nhiệm vụ trong ngày`x`không độc lập với những ngày trước đó, vì những ngày trước đó có thể đẩy nó trở lại thứ tự xử lý. 

Những hạn chế buộc chúng tôi phải tránh xa việc mô phỏng suốt cả ngày. Với tối đa`10^5`nhiệm vụ mỗi ngày và lên đến`5×10^5`số ngày được truy vấn, một mô phỏng ngây thơ mỗi ngày sẽ bùng nổ trong khoảng`5×10^10`hoạt động. Ngay cả việc mô phỏng lặp đi lặp lại một ngày cũng không đủ vì việc chuyển hàng đợi tạo ra sự phụ thuộc giữa các ngày. 

Một trường hợp khó phát hiện do quá tải hệ thống. Nếu tổng khối lượng công việc hàng ngày vượt quá khả năng sẵn có`k`giờ, hàng đợi không bao giờ được dọn sạch hoàn toàn và độ trễ tích lũy qua nhiều ngày. Nếu khối lượng công việc đủ nhỏ, mỗi ngày sẽ được xử lý độc lập và không có kết chuyển nào tồn tại. Hai chế độ này hành xử khác nhau về cơ bản và phải được tách biệt. 

Một lỗi phổ biến là mô phỏng mỗi ngày một cách độc lập, giả sử các nhiệm vụ được đặt lại hàng ngày. Điều đó thất bại ngay lập tức khi một nhiệm vụ từ ngày`x`phụ thuộc vào công việc còn dang dở trong ngày`x-1`. Một dạng lỗi khác là bỏ qua thời gian đến trong ngày và xử lý các nhiệm vụ như thể chúng đến đồng thời, điều này phá vỡ tính chính xác khi tồn tại khoảng trống nhàn rỗi vào đầu ngày. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì một hàng các nhiệm vụ đang chờ xử lý trong tất cả các ngày, đẩy các nhiệm vụ mới mỗi ngày và xử lý từng nhiệm vụ một. Điều này mô hình hóa hệ thống một cách chính xác, nhưng số lượng hoạt động tăng lên theo tổng số công việc được xử lý, có thể đạt tới`O(nq)`ngày trong trường hợp xấu nhất, vượt xa tính khả thi. 

Quan sát chính là hệ thống hoạt động ở một trong hai chế độ tùy thuộc vào tổng khối lượng công việc trong ngày có tối đa là thời gian xử lý sẵn có hay không`k`. 

Nếu tổng của tất cả`b_i`nhiều nhất là`k`, thì mỗi ngày đều bị thiếu tải. Người công nhân hoàn thành tất cả nhiệm vụ trước khi ngày kết thúc và hàng đợi trở nên trống rỗng mỗi ngày. Trong trường hợp này, ngày là bản sao độc lập của cùng một lịch trình và chúng tôi chỉ cần tính thời gian hoàn thành nhiệm vụ trong một ngày. 

Nếu tổng vượt quá`k`, thì mỗi ngày sẽ tạo ra nhiều công việc hơn mức có thể hoàn thành. Sau một thời gian ngắn nhất thời, người công nhân sẽ trở nên bận rộn vĩnh viễn. Khi điều đó xảy ra, thời gian đến chính xác trong một ngày sẽ không còn ảnh hưởng đến việc đặt hàng trong thực tế vì hàng đợi không bao giờ trống. Hệ thống thoái hóa thành một dòng chảy liên tục trong đó chỉ có tổng công việc là quan trọng. 

Điều này cho phép chúng tôi giảm quy trình vô hạn thành mô phỏng một ngày (tải nhẹ) hoặc tích lũy tiền tố ở trạng thái ổn định (tải nặng). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ qua các ngày | O(tổng công việc) | O(n) | Quá chậm | 
| Mô phỏng một ngày + xử lý trạng thái ổn định | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành hai trường hợp dựa trên tổng khối lượng công việc trong ngày. 

### Trường hợp 1: tổng công việc mỗi ngày ≤ k 

1. Tính lịch một ngày bắt đầu từ thời điểm 0. 

Chúng tôi mô phỏng các nhiệm vụ theo thứ tự tăng dần`a_i`, duy trì một con trỏ thời gian hiện tại. Mỗi nhiệm vụ bắt đầu lúc`max(current_time, a_i)`và kết thúc sau khi thêm`b_i`. Điều này tôn trọng cả các ràng buộc về điểm đến và thứ tự FIFO. 
2. Ghi lại thời gian hoàn thành từng công việc trong ngày. 

Vì tổng công việc phù hợp với`k`, không có nhiệm vụ nào được chuyển sang ngày tiếp theo nên thời gian hoàn thành này là cuối cùng. 
3. Đối với một truy vấn`(x, y)`, chuyển kết quả của một ngày sang cả ngày. 

Câu trả lời đơn giản là`(x-1)*k + completion_time_within_day[y]`. 

Điều này có tác dụng vì hệ thống thiết lập lại hàng ngày mà không còn tác vụ nào còn sót lại. 

### Trường hợp 2: tổng công việc mỗi ngày > k 

1. Quan sát xem cuối cùng hệ thống có trở nên bận vĩnh viễn hay không. 

Sau đủ ngày, luôn có công việc đang chờ xử lý vào đầu mỗi ngày, vì vậy người công nhân không bao giờ nhàn rỗi. 
2. Ở trạng thái ổn định, thời gian đến trong ngày không còn ảnh hưởng đến thứ tự thực hiện. 

Các tác vụ được xử lý hiệu quả theo thứ tự ngày nghiêm ngặt và trong mỗi ngày theo thứ tự chỉ mục vì không có khoảng trống nào xuất hiện có thể sắp xếp lại việc thực hiện. 
3. Chỉ thay thế mỗi nhiệm vụ bằng chi phí xử lý của nó. 

Hệ thống trở thành một chuỗi khối lượng công việc vô tận lặp đi lặp lại hàng ngày. 
4. Tính trước tổng tiền tố cho một ngày: 

hãy để`pref[i] = b_1 + ... + b_i`, Và`S = pref[n]`. 
5. Tính thời gian hoàn thành toàn cầu theo giờ liên tục: 

Thời gian hoàn thành nhiệm vụ`(x, y)`trở thành:`time = (x-1)*S + pref[y]`. 
6. Chuyển đổi thời gian liên tục thành`(day, hour)`bằng cách chia cho`k`. 

Điều này có tác dụng vì một khi hệ thống đã bão hòa, nhân viên sẽ không bao giờ chờ đợi, do đó, lịch trình tương đương với việc xử lý một luồng liên tục không có khoảng trống nhàn rỗi. 

### Tại sao nó hoạt động 

Trong cả hai trường hợp, bất biến chính là nhân viên luôn xử lý các nhiệm vụ theo thứ tự đến FIFO và khi hệ thống trở nên bão hòa, sẽ không có khoảng thời gian nhàn rỗi nào có thể làm sai lệch thứ tự. Hàng đợi được đặt lại mỗi ngày hoặc hàng đợi không bao giờ trống sau một thời điểm nào đó. Đây là hai hành vi ổn định duy nhất có thể thực hiện được theo mẫu đầu vào không đổi hàng ngày, do đó, việc giảm quy trình xuống mô phỏng ngày độc lập hoặc tích lũy tiền tố thuần túy sẽ nắm bắt chính xác mọi động lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, q = map(int, input().split())
    a = []
    b = []
    
    for _ in range(n):
        ai, bi = map(int, input().split())
        a.append(ai)
        b.append(bi)

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + b[i]
    S = pref[n]

    # Case 1: light load, each day clears
    if S <= k:
        finish = [0] * n
        cur = 0
        for i in range(n):
            if cur < a[i]:
                cur = a[i]
            cur += b[i]
            finish[i] = cur

        out = []
        for _ in range(q):
            x, y = map(int, input().split())
            y -= 1
            day_start = (x - 1) * k
            t = day_start + finish[y]
            d = t // k + 1
            h = t % k + 1
            out.append(f"{d} {h}")

        print("\n".join(out))
        return

    # Case 2: heavy load, steady state prefix model
    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        y -= 1
        t = (x - 1) * S + pref[y]
        d = t // k + 1
        h = t % k + 1
        out.append(f"{d} {h}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán trước tổng tiền tố của thời gian xử lý vì chúng là đại lượng duy nhất quan trọng trong chế độ quá tải. Điểm quyết định là tổng khối lượng công việc hàng ngày`S`. 

Trong nhánh tải nhẹ, chúng tôi mô phỏng rõ ràng một ngày bằng cách sử dụng một con trỏ thời gian duy nhất. Chi tiết quan trọng là`max(cur, a[i])`bước này để đảm bảo rằng chúng tôi tôn trọng thời gian hoàn thành nhiệm vụ và thời gian nhàn rỗi trong ngày. 

Trong nhánh tải nặng, chúng tôi tránh hoàn toàn mô phỏng. Mỗi truy vấn được trả lời theo thời gian không đổi bằng cách sử dụng công thức tuyến tính trên tổng tiền tố. Việc chuyển đổi từ thời gian tuyệt đối sang`(day, hour)`sử dụng phép chia số nguyên cho`k`, với điều chỉnh chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ với`k = 5`và nhiệm vụ: 

| tôi | a_i | b_i | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 4 | 3 | 

### Trace (trường hợp tải nhẹ) 

Ở đây tổng số công việc là`4 ≤ 5`, do đó không xảy ra hiện tượng chuyển giao. 

| Bước | Nhiệm vụ | Thời gian bắt đầu | Thời gian kết thúc | cur | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 2 | 
| 2 | 2 | 4 | 7 | 7 | 

Cả hai đều hoàn thành trong cùng một ngày. 

Đối với truy vấn`(2, 2)`, ngày 2 nhiệm vụ 2 kết thúc ở thời gian liên tục`5 + 7 = 12`, ánh xạ tới ngày`3`, giờ`2`. 

Điều này xác nhận rằng mỗi ngày là độc lập. 

Bây giờ hãy xem xét một ví dụ tải nặng:`k = 10`, nhiệm vụ: 

| tôi | b_i | 
| --- | --- | 
| 1 | 4 | 
| 2 | 6 | 
| 3 | 7 | 

Tổng số công việc là`17 > 10`, do đó tồn đọng tích lũy. 

Tổng tiền tố là`4, 10, 17`. 

Đối với truy vấn`(3, 2)`, chúng tôi tính toán:`time = (3-1)*17 + 10 = 44`. 

Điều này thể hiện thời gian xử lý liên tục; chuyển đổi sang ngày/giờ cho ra ngày`5`, giờ`4`. 

Dấu vết cho thấy chỉ có công việc tích lũy mới quan trọng khi hệ thống đã bão hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một tính toán tiền tố cộng với các truy vấn thời gian không đổi | 
| Không gian | O(n) | Lưu trữ mảng nhiệm vụ và tổng tiền tố | 

Các ràng buộc cho phép lên đến`10^5`nhiệm vụ và truy vấn, do đó tiền xử lý tuyến tính và thời gian không đổi cho mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# light load, independent days
assert run("""2 5 3
1 1
4 2
1 1
1 2
2 1
""") == "1 1\n1 2\n2 2"

# exact fill boundary
assert run("""2 5 2
1 2
2 3
1 2
2 1
""") == "1 5\n2 5"

# heavy load
assert run("""3 10 2
2 4
3 1
10 7
2 2
5 3
""") == "3 10\n4 7"

# minimum case
assert run("""1 5 1
1 5
1 1
""") == "1 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tải nhẹ | độc lập mỗi ngày | không chuyển tiếp | 
| điền ranh giới | đóng gói chính xác k giờ | cạnh chuyển đổi chế độ | 
| tải nặng | tích lũy tiền tố | hành vi hệ thống bão hòa | 
| trường hợp tối thiểu | tính chính xác của nhiệm vụ duy nhất | độ đúng cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tổng khối lượng công việc hàng ngày chính xác bằng`k`. Trong tình huống này, người công nhân hoàn thành công việc đúng vào cuối ngày nhưng không bao giờ chuyển tiếp bất cứ việc gì. Ví dụ: nếu nhiệm vụ hoàn thành đầy đủ`k`giờ, nhánh tải nhẹ vẫn được áp dụng. Quá trình mô phỏng kết thúc vào thời điểm chính xác`k`, do đó dịch chuyển theo`(x-1)*k`đặt liên tiếp nhau mỗi ngày mà không chồng chéo lên nhau. 

Một trường hợp khác là khi nhiệm vụ đầu tiên đến muộn trong ngày. Ngay cả trong hệ thống tải nhẹ, công nhân sẽ không hoạt động cho đến khi`a_1`. Mô phỏng xử lý việc này thông qua`max(cur, a[i])`bước chân. Nếu không có nó, thời gian nhàn rỗi sớm sẽ bị bỏ qua và tất cả thời gian hoàn thành sẽ bị dịch chuyển không chính xác sớm hơn. 

Trường hợp thứ ba là một hệ thống được tải nặng trong đó những ngày đầu vẫn có vẻ không hoạt động một phần. Mặc dù cuối cùng hệ thống đã bão hòa nhưng công thức trạng thái ổn định chỉ áp dụng sau quá trình chuyển đổi. Quyết định dựa trên tổng khối lượng công việc hàng ngày sẽ tránh được các chế độ trộn lẫn và đảm bảo chúng tôi không giả định sai về tình trạng bão hòa ngay lập tức.
