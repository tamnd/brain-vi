---
title: "CF 102471G - Hạnh phúc"
description: "Pang là một trong n đội tham gia cuộc thi ICPC 10 vấn đề. n - 1 đội còn lại có kết quả chung cuộc cố định. Pang biết một tập hợp con của mười vấn đề và việc giải quyết một vấn đề đã biết sẽ mất một khoảng thời gian cố định và phải chịu một số lần gửi bị từ chối cố định trước khi được chấp nhận."
date: "2026-08-09T15:53:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "G"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 670
verified: true
draft: false
---

[CF 102471G - Hạnh phúc](https://codeforces.com/problemset/problem/102471/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11m 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bàng là một trong`n`các đội tham gia cuộc thi ICPC 10 vấn đề. Cái khác`n - 1`các đội đã ấn định kết quả cuối cùng. Pang biết một tập hợp con của mười vấn đề và việc giải quyết một vấn đề đã biết sẽ mất một khoảng thời gian cố định và phải chịu một số lần gửi bị từ chối cố định trước khi được chấp nhận. 

Pang phải giải quyết hết vấn đề này đến vấn đề khác. Nếu một vấn đề xảy ra`x`phút, thì việc gửi được chấp nhận sẽ diễn ra vào thời điểm hiện tại trôi qua cộng thêm`x`. Thời gian hoàn thành đó được đưa vào danh sách thời gian giải pháp của nhóm. Thời gian hoàn thành như nhau, cộng thêm`20`phút cho mỗi lần từ chối trước đó về vấn đề đó sẽ tính vào tổng số hình phạt của đội. Pang có thể dừng lại bất cứ lúc nào, nhưng mọi lần gửi được chấp nhận phải diễn ra không muộn hơn một phút`300`. 

Thứ tự ảnh hưởng đến kết quả của Pang vì các vấn đề sau này có thời gian hoàn thành lớn hơn. Thứ hạng cuối cùng được xác định trước tiên bởi số lượng bài toán đã giải được, sau đó là tổng số điểm phạt và cuối cùng là danh sách thời gian hoàn thành giảm dần. Pang giành chiến thắng hòa hoàn toàn trước mọi đội khác. 

Hạnh phúc là một chức năng của cấp bậc này, cùng với một số phần thưởng độc lập. Huy chương chỉ phụ thuộc vào thứ hạng. Mỗi vấn đề góp phần`800`nếu thời gian hoàn thành vấn đề đó của Pang không muộn hơn thời gian hoàn thành vấn đề đó của mọi đội khác. Ngoài ra còn có một`700`thưởng khi có thời gian giải sớm nhất trong toàn bộ cuộc thi và`500`tiền thưởng khi có cái mới nhất. Số lần bằng nhau được tính là Pang giành được phần thưởng tương ứng. 

Đầu vào chứa`n - 1`mô tả đội cố định, sau đó là mô tả của Pang. Trạng thái đã giải quyết chứa thời gian được chấp nhận và số lần từ chối, trong khi`-`nghĩa là vấn đề chưa được giải quyết. Đầu ra là mức độ hạnh phúc tối đa mà Pang có thể đạt được bằng cách chọn cả những vấn đề đã biết để giải quyết và thứ tự của chúng. Tuyên bố chính thức và mẫu có sẵn trên Codeforces. 

Số lượng vấn đề nhỏ là hạn chế chính. Chỉ có mười vấn đề, vì vậy lập trình động tập hợp con là thực tế. Phần lớn có khả năng là`n`, có thể đạt tới`300`. Điều đó có nghĩa là chúng ta nên tránh làm công việc tỷ lệ thuận với`n`cho hàng triệu lịch trình có thể có của Pang. Một giải pháp xung quanh`O(n * 2^10 * poly(10))`đủ nhỏ, trong khi liệt kê hàng triệu lịch trình và so sánh từng lịch trình với tất cả`300`các đội thì không. 

Có một số trường hợp đặc biệt quan trọng vì chúng thay đổi cách tính thứ hạng hoặc tiền thưởng. Pang có thể không giải quyết được vấn đề gì cả. Ví dụ: với mười đội và mọi trạng thái bằng`-`, Pang không có vấn đề gì được giải quyết và ràng buộc mọi đội, vì vậy Pang được xếp hạng`1`và nhận được`5000`niềm hạnh phúc. Một giải pháp cho rằng Pang phải giải quyết vấn đề nào đó sẽ bỏ lỡ giải pháp tối ưu. 

Ranh giới vào đúng phút`300`cũng rất đáng kể. Hãy xem xét 10 đội trong đó mỗi đội chỉ giải quyết được 1 vấn đề tại một thời điểm`300`không có lượt chạy nào bị từ chối và Pang có trạng thái tương tự. Pang là cấp bậc`1`, giải pháp được xem xét đầu tiên vì mối quan hệ có lợi cho Pang, và giải pháp vừa là giải pháp sớm nhất vừa là giải pháp mới nhất. Câu trả lời là`5000 + 1200 + 800 + 700 + 500 = 8200`. điều trị`300`bị cấm thay vì được phép sẽ loại bỏ giải pháp duy nhất. 

Hòa cũng phải nghiêng về Pang khi tính thứ hạng. Nếu thứ hạng chính của Pang giống với một số đội khác thì những đội đó không được tính là cao hơn. Đây là lý do tại sao việc triển khai sử dụng giới hạn dưới trong danh sách được sắp xếp của các nhóm khác. Sử dụng giới hạn trên sẽ đặt Pang phía sau các đội bằng nhau. 

Hình phạt từ chối phải được tách biệt khỏi danh sách thời gian giải quyết. Một vấn đề được giải quyết vào thời điểm đó`100`với ba lần chạy bị từ chối đóng góp`160`bị phạt hoàn toàn, nhưng thời gian giải quyết việc so sánh từ điển vẫn còn dài`100`. Việc nhầm lẫn hai giá trị này có thể thay đổi thứ hạng ngay cả khi số lượng bài toán được giải là chính xác. 

Cuối cùng, phần thưởng cho giải pháp mới nhất dựa trên thời gian trôi qua cuối cùng của Pang, là tổng thời gian giải quyết của tất cả các vấn đề đã chọn. Nó không chỉ dựa trên độ dài của vấn đề cuối cùng. Phần thưởng cho giải pháp sớm nhất cũng tương tự dựa trên thời gian hoàn thành đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi trình tự có thể có của các vấn đề đã biết. Một chuỗi có thể dừng sau bất kỳ độ dài nào vì Pang được phép dừng giải. Với 10 bài toán, số dãy có thứ tự khác rỗng là`P(10,1) + P(10,2) + ... + P(10,10) = 9,864,100`. 

Với mỗi chuỗi như vậy, chúng tôi có thể tính toán điểm xếp hạng của Pang và so sánh nó với mọi đội khác. Trong trường hợp xấu nhất, với`n = 300`, điều đó có nghĩa là khoảng`9,864,100 * 299 = 2,949,365,900`so sánh nhóm thậm chí trước khi xem xét chi phí so sánh danh sách giải pháp theo thời gian. Bạo lực là đúng vì mọi lịch trình pháp lý đều xuất hiện rõ ràng, nhưng nó quá đắt. 

Quan sát hữu ích đầu tiên là các đội khác không bao giờ thay đổi. Chúng ta có thể sắp xếp tất cả các đội khác một lần theo quy tắc xếp hạng chính xác. Sau đó, thứ hạng của Pang có thể được tìm thấy bằng tìm kiếm nhị phân thay vì so sánh với mọi đội. 

Thứ hạng của một đội có thể được biểu thị bằng một bộ dữ liệu:`(-solved_count, total_penalty, descending_solution_times)`. 

Bộ dữ liệu nhỏ hơn đại diện cho các đội tốt hơn. Việc phủ định số lượng đã giải quyết sẽ biến "nhiều vấn đề được giải quyết tốt hơn" thành thứ tự bộ dữ liệu thông thường. Danh sách thời gian giải quyết giảm dần đã có hướng từ điển chính xác. 

Thứ hạng của Pang khi đó là một cộng với số đội khác có bộ dữ liệu nhỏ hơn hoàn toàn. Vì Pang thắng hòa,`bisect_left`đưa ra chính xác vị trí cần thiết. 

Điều đó loại bỏ yếu tố`n`từ đánh giá lịch trình, nhưng việc liệt kê gần mười triệu lịch trình vẫn là không cần thiết. 

Quan sát thứ hai là số lượng vấn đề chỉ có mười, do đó tập hợp các vấn đề đã được giải quyết có thể được biểu diễn bằng mặt nạ bit. Đối với một tập hợp con cố định, thời gian trôi qua cuối cùng là cố định vì nó đơn giản là tổng thời lượng giải. Số lượng xếp hạng phụ thuộc vào thứ tự duy nhất là tổng thời gian hoàn thành. 

Có một tài sản quan trọng khác. Đối với một tập hợp con cố định, số phần thưởng đầu tiên có vấn đề có thể được sử dụng dưới dạng thứ nguyên DP nhỏ. Nếu hai lệnh giải quyết chính xác cùng một tập hợp con, nhận được cùng số phần thưởng cho vấn đề đầu tiên và có tổng số hình phạt khác nhau, thì lệnh có hình phạt nhỏ hơn luôn được ưu tiên hơn. Mọi thành phần hạnh phúc tùy thuộc vào thứ hạng đều đơn điệu với thứ hạng, vì vậy tổng hình phạt tệ hơn không bao giờ có thể bù đắp cho thứ tự tốt hơn khi tất cả các phần thưởng khác đều giống hệt nhau. 

Nếu các hình phạt cũng bằng nhau thì chỉ có danh sách thời gian hoàn thành giảm dần nhỏ hơn về mặt từ điển mới quan trọng. Do đó, một trạng thái DP chỉ cần cặp tốt nhất bao gồm tổng số hình phạt và danh sách thời gian giải quyết. 

Phần thưởng duy nhất phụ thuộc vào chính vấn đề đầu tiên là`700`phần thưởng giải pháp sớm nhất. Chúng ta có thể xử lý vấn đề này một cách rõ ràng bằng cách khắc phục sự cố đầu tiên và chạy tập hợp con DP cho từng sự cố đầu tiên có thể xảy ra. Chỉ có mười sự lựa chọn. Sau khi vấn đề đầu tiên được khắc phục, thời gian hoàn thành và phần thưởng giải pháp sớm nhất sẽ được biết ở mọi tiểu bang. 

DP kết quả khám phá các tập hợp con thay vì các chuỗi được sắp xếp. Quá trình chuyển đổi nối thêm một vấn đề chưa được giải quyết. Nếu thời gian trôi qua hiện tại là`T`và vấn đề`j`mất`x[j]`, thời gian hoàn thành của nó trở thành`T + x[j]`. Việc chuyển đổi chỉ hợp pháp nếu giá trị này lớn nhất`300`. 

Phần thưởng đầu tiên có vấn đề đặc biệt thuận tiện. Đối với mọi vấn đề, hãy tính toán trước thời gian giải quyết sớm nhất mà bất kỳ nhóm nào khác đạt được. Nếu Pang hoàn thành bài toán đó không muộn hơn ngưỡng này, anh ấy sẽ nhận được`800`thưởng. Trường hợp bình đẳng thuộc về Pang. 

Đối với phần thưởng toàn cầu, hãy tính toán trước thời gian giải quyết tối thiểu và tối đa giữa tất cả các nhóm khác. Khi Pang giải quyết được ít nhất một vấn đề, thời gian hoàn thành đầu tiên của anh ấy sẽ quyết định`700`tiền thưởng và thời gian hoàn thành cuối cùng của anh ấy quyết định`500`thưởng. 

Phương pháp vũ phu hoạt động vì nó xem xét trực tiếp mọi đơn hàng. Nó không thành công vì cùng một tập hợp con chứa nhiều tiền tố dư thừa. Quan sát cho thấy tất cả thông tin phụ thuộc vào thứ tự có liên quan có thể được tóm tắt bằng một tập hợp con, số lượng phần thưởng nhỏ và khóa xếp hạng tốt nhất cho phép chúng tôi thu gọn các lịch trình đó thành chỉ còn vài nghìn trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(P(10,1)+...+P(10,10)) * O(n)`|`O(10)`| Quá chậm | 
| Tối ưu |`O(10 * 2^10 * 10 * 11 * log n)`|`O(2^10 * 11 * 10)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích mười trạng thái vấn đề của mỗi đội khác và xây dựng chìa khóa xếp hạng của đội đó. Đếm các vấn đề đã được giải quyết của nó, tính tổng số tiền phạt của nó là`t + 20w`cho mọi vấn đề được giải quyết và lưu trữ thời gian giải quyết theo thứ tự giảm dần. Sắp xếp tất cả`n - 1`phím. 
2. Trong khi phân tích cú pháp của các đội khác, hãy tính cả bốn ngưỡng. Đối với mỗi vấn đề, hãy lưu trữ thời gian giải quyết nhỏ nhất mà bất kỳ nhóm nào khác đã đạt được. Lưu trữ thời gian giải nhỏ nhất trong toàn bộ cuộc thi và thời gian giải lớn nhất trong toàn bộ cuộc thi. Nếu không ai giải quyết được một vấn đề cụ thể thì ngưỡng của nó có thể được đặt thành`301`, bởi vì mọi giải pháp Pang hợp pháp nhiều nhất là`300`. Nếu không ai giải quyết được gì, hãy sử dụng`301`cho mức tối thiểu toàn cầu và`0`cho mức tối đa toàn cầu. 
3. Đánh giá khả năng Pang không giải quyết được gì. Chìa khóa xếp hạng của nó là`(0, 0, ())`và không áp dụng tiền thưởng theo thời gian. Ứng viên này phải được xem xét vì việc giải quyết một vấn đề thực sự có thể làm giảm thứ hạng của Pang mà không nhất thiết phải bù đắp bằng tiền thưởng. 
4. Khắc phục một sự cố đã biết`first`là vấn đề đầu tiên của Pang. Khởi tạo một tập con DP chỉ chứa bài toán này. Thời gian trôi qua là thời gian giải quyết, phần phạt là thời gian hoàn thành cộng thêm`20`lần số lần từ chối của nó và danh sách thời gian giải pháp của nó chứa thời gian hoàn thành đó. 
5. Đối với mỗi mặt nạ có chứa`first`, duy trì một trạng thái cho mỗi số phần thưởng đầu tiên có thể có. Tiểu bang lưu trữ tổng số hình phạt nhỏ nhất có thể đạt được với mặt nạ và số tiền thưởng đó. Nếu hai trạng thái có mức phạt bằng nhau, hãy giữ bộ dữ liệu về thời gian hoàn thành giảm dần nhỏ hơn về mặt từ điển vì trạng thái đó có thứ hạng tốt hơn. 
6. Thử thêm mọi vấn đề đã biết chưa có trong mặt nạ. Thời gian hoàn thành mới là tổng thời lượng của tập hợp con hiện tại cộng với thời lượng của vấn đề mới. Từ chối quá trình chuyển đổi nếu điều này vượt quá`300`, vì Pang không thể gửi sau khi cuộc thi kết thúc. 
7. Thêm hình phạt từ chối của vấn đề mới và thời gian hoàn thành vào tổng hình phạt. Nếu thời gian hoàn thành của nó không muộn hơn thời gian tốt nhất của nhóm khác cho vấn đề đó, hãy tăng số tiền thưởng cho vấn đề đầu tiên lên một. Vì thời gian hoàn thành mới muộn hơn mọi thời gian hoàn thành trước đó, nên hãy thêm nó vào bộ dữ liệu thời gian giải pháp giảm dần. 
8. Sau khi tất cả các mặt nạ đã được xử lý cho sự cố đầu tiên đã được khắc phục, hãy đánh giá mọi trạng thái có thể tiếp cận được như một điểm dừng có thể xảy ra. Khóa xếp hạng của nó được hình thành trực tiếp từ kích thước tập hợp con, hình phạt được lưu trữ và bộ dữ liệu theo thời gian giải pháp giảm dần được lưu trữ. Sử dụng`bisect_left`trong danh sách sắp xếp của các đội khác để đạt được thứ hạng của Pang. 
9. Thêm hạnh phúc theo cấp bậc và hạnh phúc huy chương. Thêm vào`800`lần số tiền thưởng đầu tiên được lưu trữ trong vấn đề. Vì sự cố đầu tiên đã được khắc phục, hãy thêm`700`khi thời gian hoàn thành không muộn hơn thời gian giải tối thiểu chung của các đội khác. Thêm vào`500`khi thời gian trôi qua cuối cùng ít nhất bằng thời gian giải tối đa toàn cục của các đội khác. 
10. Lặp lại DP cho mọi vấn đề đầu tiên có thể xảy ra và giữ giá trị hạnh phúc lớn nhất. Mọi lịch trình Pang không trống hợp pháp đều có chính xác một vấn đề đầu tiên, do đó nó xuất hiện ở đúng một trong mười lần chạy DP này. 

Tính bất biến của tính chính xác là, đối với mọi vấn đề đầu tiên cố định, mặt nạ tập hợp con và số phần thưởng đầu tiên của vấn đề, trạng thái DP được giữ lại là trạng thái xếp hạng tốt nhất có thể có trong số tất cả các đơn hàng tạo ra chính xác thông tin đó. Hình phạt lớn hơn không bao giờ có thể cải thiện thứ hạng, trong khi hình phạt tương đương chỉ được quyết định theo danh sách thời gian giải pháp, vì vậy việc loại bỏ mọi trạng thái khác là an toàn. Mỗi lịch trình pháp lý được xây dựng lần lượt từng vấn đề thông qua những chuyển đổi này và mọi điểm dừng đều được đánh giá. Khóa xếp hạng khớp chính xác với quy tắc xếp hạng của cuộc thi, với`bisect_left`thực hiện lợi thế của Pang trong quan hệ. Vì mọi lịch trình có thể đều được biểu diễn và mọi trạng thái bị loại bỏ đều bị chi phối đối với tất cả các thành phần hạnh phúc còn lại, nên mức hạnh phúc được đánh giá tối đa là mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def parse_team(line):
    result = []
    for item in line.strip().split(","):
        if item == "-":
            result.append(None)
        else:
            t, w = map(int, item.split())
            result.append((t, w))
    return result

def solve_parsed(n, rows):
    other_rows = rows[:n - 1]
    pang_row = rows[n - 1]

    other_keys = []

    first_limit = [301] * 10
    global_min = 301
    global_max = 0

    for line in other_rows:
        team = parse_team(line)

        solved = 0
        penalty = 0
        times = []

        for j, status in enumerate(team):
            if status is None:
                continue

            t, w = status
            solved += 1
            penalty += t + 20 * w
            times.append(t)

            if t < first_limit[j]:
                first_limit[j] = t
            if t < global_min:
                global_min = t
            if t > global_max:
                global_max = t

        times.sort(reverse=True)
        other_keys.append((-solved, penalty, tuple(times)))

    other_keys.sort()

    pang = parse_team(pang_row)

    x = [None] * 10
    y = [0] * 10
    known = []

    for i, status in enumerate(pang):
        if status is not None:
            x[i], y[i] = status
            known.append(i)

    # Rank of Pang. Pang wins a complete tie, so equal keys
    # must be placed after Pang, which is exactly bisect_left.
    def get_rank(solved, penalty, times):
        key = (-solved, penalty, times)
        return bisect_left(other_keys, key) + 1

    def rank_happiness(rank):
        h = 5000 // rank
        q = n // 10

        if rank <= q:
            h += 1200
        elif rank <= 3 * q:
            h += 800
        elif rank <= 6 * q:
            h += 400

        return h

    # Pang solves nothing.
    best = rank_happiness(get_rank(0, 0, ()))

    size = 1 << 10

    # Sum of solving durations for every mask.
    sum_time = [0] * size
    for mask in range(1, size):
        bit = mask & -mask
        j = bit.bit_length() - 1
        sum_time[mask] = sum_time[mask ^ bit]
        if x[j] is not None:
            sum_time[mask] += x[j]

    for first in known:
        first_mask = 1 << first
        first_completion = x[first]
        first_bonus = 1 if first_completion <= first_limit[first] else 0
        first_penalty = first_completion + 20 * y[first]

        # dp[mask] is a dictionary:
        # bonus_count -> (minimum_penalty, descending_solution_times)
        dp = [None] * size
        dp[first_mask] = {
            first_bonus: (first_penalty, (first_completion,))
        }

        for mask in range(size):
            states = dp[mask]
            if not states or not (mask & first_mask):
                continue

            elapsed = sum_time[mask]

            for bonus_count, state in list(states.items()):
                penalty, times = state

                for j in known:
                    bit = 1 << j
                    if mask & bit:
                        continue

                    new_elapsed = elapsed + x[j]
                    if new_elapsed > 300:
                        continue

                    add_bonus = 1 if new_elapsed <= first_limit[j] else 0
                    new_bonus = bonus_count + add_bonus

                    new_penalty = penalty + new_elapsed + 20 * y[j]
                    new_times = (new_elapsed,) + times

                    new_mask = mask | bit
                    if dp[new_mask] is None:
                        dp[new_mask] = {}

                    old = dp[new_mask].get(new_bonus)

                    if old is None or (
                        new_penalty < old[0]
                        or (
                            new_penalty == old[0]
                            and new_times < old[1]
                        )
                    ):
                        dp[new_mask][new_bonus] = (
                            new_penalty,
                            new_times,
                        )

        # Every reachable state is a legal point at which Pang may stop.
        for mask in range(size):
            states = dp[mask]
            if not states:
                continue

            solved = mask.bit_count()
            final_elapsed = sum_time[mask]

            for bonus_count, (penalty, times) in states.items():
                rank = get_rank(solved, penalty, times)

                happiness = rank_happiness(rank)
                happiness += 800 * bonus_count

                # The first problem is fixed in this DP run.
                if first_completion <= global_min:
                    happiness += 700

                if final_elapsed >= global_max:
                    happiness += 500

                if happiness > best:
                    best = happiness

    return best

def main():
    n = int(input())
    rows = [input().strip() for _ in range(n)]
    print(solve_parsed(n, rows))

if __name__ == "__main__":
    main()
```Mã phân tích cú pháp chuyển đổi mọi trạng thái đã giải quyết thành`(time, rejected_runs)`và đại diện`-`qua`None`. Đối với các đội khác, khóa xếp hạng được xây dựng ngay sau khi phân tích cú pháp. Danh sách giải pháp-thời gian được sắp xếp theo thứ tự giảm dần vì đó chính xác là thứ tự được sử dụng trong hiệp đấu cuối cùng. 

Bốn ngưỡng được thu thập trong cùng một lần vượt qua, do đó không cần phải quét riêng từng nhóm. Một ngưỡng của`301`là an toàn vì Pang không bao giờ có thể có thời gian hoàn thành pháp lý lớn hơn`300`. 

các`get_rank`chức năng là một trong những phần tinh tế nhất. Tuple sử dụng`-solved`bởi vì phép so sánh bộ dữ liệu thông thường của Python xử lý các giá trị nhỏ hơn sẽ tốt hơn. Sau khi sắp xếp tất cả các đội khác,`bisect_left`trả về số lượng khóa nhỏ hơn. Các ván bằng nhau không được tính là tốt hơn vì Pang thắng các ván đó. 

Mảng thời lượng tập hợp con cho phép mọi chuyển đổi có được thời gian đã trôi qua hiện tại trong thời gian không đổi. Để chuyển từ`mask`ĐẾN`mask | (1 << j)`, thời gian hoàn thành mới chỉ đơn giản là`sum_time[mask] + x[j]`. 

DP chỉ lưu trữ hình phạt tốt nhất cho mỗi lần tính tiền thưởng. Thuật ngữ từ chối`20 * y[j]`được thêm vào khi có vấn đề`j`được giải quyết, đồng thời thời gian hoàn thành của nó cũng được cộng thêm vì đó là một phần của hình phạt cuộc thi. Bộ dữ liệu về thời gian hoàn thành không bao gồm các hình phạt từ chối. 

Việc chuẩn bị trước thời gian hoàn thành mới cho bộ dữ liệu là chính xác vì mọi vấn đề sau này đều có thời gian hoàn thành lớn hơn. Vì vậy, nếu thời gian hoàn thành theo thứ tự thời gian là`t1, t2, t3`, danh sách xếp hạng là`(t3, t2, t1)`. 

các`300`sử dụng ranh giới`<=`, không`<`. Một vấn đề hoàn thành chính xác vào phút`300`là hợp pháp. Phần thưởng sớm nhất và mới nhất cũng sử dụng so sánh toàn diện vì Pang thắng hòa. 

Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý tràn. Dù sao thì hình phạt lớn nhất cũng rất nhỏ so với phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

Mẫu chính thức cho thấy Pang biết sáu bài toán, nhưng thời gian giải của chúng chỉ khiến một số lịch trình hai bài toán trở nên khả thi trong vòng.`300`phút. Một lịch trình tối ưu giải quyết vấn đề 1 trước tiên, lấy`38`phút, rồi đến vấn đề 8, lấy một vấn đề khác`254`phút. Các trạng thái DP quan trọng cho lịch trình đó được hiển thị bên dưới. Đầu ra mẫu chính thức là`1800`. 

| Bước | Đã giải quyết vấn đề | Đã qua | Phạt đền | Thời gian giải quyết giảm dần | Phần thưởng đầu tiên có vấn đề | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu |`{}`|`0`|`0`|`()`|`0`| 
| Giải quyết vấn đề 1 |`{1}`|`38`|`238`|`(38,)`|`1`| 
| Giải bài 8 |`{1,8}`|`292`|`670`|`(292,38)`|`1`| 

Các đội khác gồm có một đội có hai vấn đề được giải quyết và một quả phạt đền`397`, nên quả phạt đền của Pang`670`để anh ta ở cấp bậc`10`trong số mười đội. Giải pháp sớm nhất khác là vào phút`4`, nên Pang không nhận được`700`phần thưởng giải pháp sớm nhất. Giải pháp mới nhất khác là vào phút`290`, trong khi Pang kết thúc ở`292`, vì vậy anh ta nhận được`500`phần thưởng giải pháp mới nhất. Vấn đề 1 được giải quyết trước tiên vì Pang`38`sớm hơn giải pháp của mọi đội khác cho vấn đề đó. Kết quả hạnh phúc là`500 + 800 + 500 = 1800`. 

Ví dụ thứ hai tách biệt chính xác`300`ranh giới. Giả sử cả 9 đội còn lại chỉ giải được bài toán 1 trong một phút.`300`, và Pang cũng có cùng một vấn đề. 

| Bước | Đã giải quyết vấn đề | Đã qua | Xếp hạng | Phần thưởng đầu tiên có vấn đề | Tiền thưởng sớm nhất | Tiền thưởng mới nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu |`{}`|`0`|`1`|`0`|`0`|`0`| 
| Giải quyết vấn đề 1 |`{1}`|`300`|`1`|`1`|`1`|`1`| 

Pang giành chiến thắng trong bảng xếp hạng hoàn toàn vì chìa khóa của anh ấy giống hệt chìa khóa của mọi đội khác và dây buộc thuộc về Pang. Việc hoàn thành chính xác`300`là hợp pháp và sự bình đẳng là đủ cho cả ba khoản tiền thưởng theo thời gian. Hạnh phúc cuối cùng là`5000 + 1200 + 800 + 700 + 500 = 8200`. 

Dấu vết này chứng minh tại sao DP phải cho phép dừng ở trạng thái có thời gian trôi qua chính xác`300`, tại sao xếp hạng phải sử dụng`bisect_left`và tại sao tất cả các so sánh thời gian đều mang tính bao hàm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * 10 + 10 * 2^10 * 10 * 11 * log n)`| Phân tích cú pháp là tuyến tính về số lượng trạng thái. Mỗi vấn đề đầu tiên chạy một tập hợp con DP với tối đa mười lần chuyển đổi và mười một trạng thái đếm tiền thưởng, đồng thời mọi trạng thái được giữ lại đều có thể được xếp hạng bằng tìm kiếm nhị phân. | 
| Không gian |`O(n * 10 + 2^10 * 11 * 10)`| Các khóa xếp hạng nhóm cố định được lưu trữ, trong khi mỗi trạng thái DP lưu trữ một hình phạt và một bộ độ dài theo thời gian giải pháp tối đa là mười. | 

Chỉ với mười vấn đề,`2^10 = 1024`. Ngay cả sau khi nhân với mười vấn đề đầu tiên có thể xảy ra và mười một số tiền thưởng có thể xảy ra, không gian trạng thái vẫn còn nhỏ. Cuộc thi có thể chứa`300`các đội, nhưng những đội đó được xử lý một lần và sau đó được thể hiện bằng các khóa xếp hạng được sắp xếp. Giải pháp thoải mái phù hợp với`2`thứ hai và`256 MB`giới hạn. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp được lưu dưới dạng`solution.py`, phơi bày`solve_parsed`chức năng hiển thị ở trên.```
from solution import solve_parsed

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    n = int(lines[0])
    return str(solve_parsed(n, lines[1:]))

# Official sample
sample1 = """\
10
233 1,-,-,7 7,257 4,173 5,117 1,-,-,85 3
-,231 0,167 0,257 7,-,-,122 4,283 0,215 4,-
41 1,-,290 8,-,-,-,-,246 7,120 3,184 9
142 8,243 7,69 0,-,41 9,-,279 1,264 4,-,74 9
53 8,-,187 9,60 1,48 8,99 10,-,-,55 7,259 5
250 0,-,-,-,166 0,16 3,-,82 4,73 0,184 3
-,-,-,-,105 3,-,-,-,152 4,-
-,84 5,98 8,-,120 8,241 3,94 1,-,28 7,109 8
280 6,246 5,58 9,-,-,-,-,-,-,-
38 10,-,227 10,187 9,182 1,-,203 9,254 7,-,-
"""
assert run(sample1) == "1800", "official sample"

# Minimum-size contest, everyone solves nothing.
sample2 = """\
10
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
-,-,-,-,-,-,-,-,-,-
"""
assert run(sample2) == "5000", "zero solved problems"

# Exact 300-minute boundary, with a complete tie.
sample3 = """\
10
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
300 0,-,-,-,-,-,-,-,-,-
"""
assert run(sample3) == "8200", "exactly 300 and Pang wins ties"

# Ranking is worse, but time bonuses and one first-solved bonus remain.
sample4 = """\
10
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
120 0,80 0,-,-,-,-,-,-,-,-
50 0,100 0,-,-,-,-,-,-,-,-
"""
assert run(sample4) == "2500", "order-dependent first-solved bonus"

# Maximum n, repeated Pang durations, and the 300-minute final boundary.
other = "-,-,-,-,-,-,-,-,-,-"
pang = "30 0,30 0,30 0,30 0,30 0,30 0,30 0,30 0,30 0,30 0"

sample5 = "300\n" + "\n".join([other] * 299 + [pang]) + "\n"
assert run(sample5) == "15400", "maximum n and repeated durations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức |`1800`| Hoàn thành xếp hạng và tương tác tiền thưởng | 
| Mười đội không giải quyết được vấn đề |`5000`| Lịch Pang trống | 
| Chín giống nhau`300`đội phút |`8200`| Chính xác`300`ranh giới và lợi thế hòa của Pang | 
| Hai vấn đề với ngưỡng`120`Và`80`|`2500`| Phần thưởng và xếp hạng được giải quyết đầu tiên phụ thuộc vào đơn hàng | 
|`n = 300`, mười vấn đề`30`mỗi |`15400`| Tối đa`n`, thời lượng lặp lại, tất cả phần thưởng của sự cố và lần cuối cùng`300`| 

## Vỏ cạnh 

Khi Pang không giải quyết được gì, thuật toán sẽ đánh giá khóa xếp hạng trống trước khi nhập bất kỳ DP nào. Dành cho 10 đội có tất cả các trạng thái bằng nhau`-`, mỗi đội đều có chìa khóa`(0, 0, ())`.`bisect_left`trả về vị trí`0`, xếp hạng Pang`1`, Và`5000`niềm hạnh phúc. Không có phần thưởng thời gian được thêm vào vì Pang không có giải pháp. Điều này ngăn việc triển khai không chính xác buộc Pang phải chọn một vấn đề đã biết. 

Để có giải pháp chính xác vào từng phút`300`, điều kiện chuyển tiếp là`new_elapsed > 300`, Vì thế`new_elapsed == 300`vẫn hợp pháp. Nếu mọi đội khác giải quyết được vấn đề 1 tại`300`và Pang cũng làm như vậy, chìa khóa xếp hạng của mọi người đều giống nhau.`bisect_left`xếp hạng Pang`1`. Việc so sánh vấn đề đầu tiên sử dụng`<=`, vậy là Pang nhận được`800`; các so sánh tối thiểu và tối đa toàn cầu cũng được bao gồm, mang lại cả`700`Và`500`. Kết quả là`8200`. 

Đối với một bảng xếp hạng có nhiều đội bằng nhau, bản thân khóa không chứa mã định danh đội. Đây là cố ý. Pang không chỉ bị ràng buộc với những đội đó, anh ấy còn được xác định là xếp trước họ. Nếu năm đội khác có cùng khóa với Pang,`bisect_left`đặt Pang trước cả năm người. sử dụng`bisect_right`sẽ đặt anh ta theo họ một cách không chính xác và cũng có thể thay đổi huy chương và hạnh phúc dựa trên cấp bậc của anh ta. 

Đối với phần thưởng giải quyết vấn đề đầu tiên, DP so sánh thời gian hoàn thành chứ không phải thời gian giải quyết thô. Giả sử Pang lần đầu tiên giải được một`50`vấn đề -phút và sau đó là một`100`-vấn đề nhỏ. Thời gian hoàn thành của họ là`50`Và`150`, không`50`Và`100`. Phần thưởng được giải quyết đầu tiên của vấn đề thứ hai phải được kiểm tra`150`. Quá trình chuyển đổi tính toán rõ ràng thời gian hoàn thành tích lũy này, do đó, các vấn đề sau này không thể vô tình nhận được phần thưởng chỉ dựa trên thời lượng giải quyết của chính chúng. 

Đối với phần thưởng cho giải pháp mới nhất, mỗi vấn đề được chọn sẽ đóng góp thời gian giải quyết của nó vào thời gian đã trôi qua cuối cùng. Nếu Pang giải quyết được vấn đề lấy`50`Và`100`phút, thời gian giải quyết cuối cùng là`150`bất kể vấn đề nào là thứ hai. DP sử dụng`sum_time[mask]`đối với giá trị này, vì vậy`500`tiền thưởng được đánh giá theo thời gian gửi thực tế cuối cùng. 

Đối với tổng số tiền phạt bằng nhau, DP so sánh các bộ dữ liệu về thời gian hoàn thành giảm dần được lưu trữ. Điều này là cần thiết vì cuộc thi sử dụng danh sách đó làm tiêu chí xếp hạng thứ ba. Ví dụ: hai lịch trình đều có thể bị phạt`300`, nhưng người ta có thể sản xuất`(200, 100)`trong khi người khác sản xuất`(190, 110)`. Cái sau tốt hơn bởi vì`190 < 200`ở vị trí khác nhau đầu tiên. Việc so sánh trạng thái sử dụng thứ tự bộ dữ liệu Python thông thường, khớp chính xác với quy tắc từ điển được yêu cầu. 

Đối với một vấn đề mà không ai khác giải quyết được, ngưỡng giải quyết đầu tiên của nó vẫn là`301`. Mỗi lần hoàn thành Pang hợp pháp tối đa là`300`, do đó Pang tự động nhận được`800`tiền thưởng cho vấn đề đó. Ý tưởng tương tự cũng áp dụng cho ngưỡng tối thiểu chung khi không có nhóm nào khác giải quyết được vấn đề gì. sử dụng`301`thay vì một trọng điểm rất lớn cũng làm cho đối số ranh giới trở nên rõ ràng. 

Đối với mức tối đa toàn cầu, ngưỡng mặc định là`0`khi không có đội nào khác giải quyết được vấn đề gì. Bất kỳ lịch trình nào của Pang không trống đều kết thúc vào thời điểm dương, do đó Pang nhận được`500`phần thưởng giải pháp mới nhất. Lịch trình trống được đánh giá riêng và không nhận được vì tiền thưởng yêu cầu ít nhất một giải pháp. 

Một chi tiết triển khai nhỏ cần lưu ý khi gửi là mẫu hiển thị trong tuyên bố được trình bày trực quan trên các dòng, trong khi đầu vào thực tế của giám khảo có chính xác một mô tả nhóm trên mỗi dòng.
