---
title: "CF 102536G - Phim Điệp Viên Chung"
description: "Chúng ta cần xây dựng một chuỗi các diễn viên. Một diễn viên là một tập hợp chính xác g diễn viên được chọn từ a diễn viên có sẵn. Lần diễn viên đầu tiên đã được cố định. Mỗi bộ phim tiếp theo phải có được bằng cách loại bỏ chính xác một diễn viên và thêm chính xác một diễn viên khác. Ngoài ra, không có dàn diễn viên nào có thể xuất hiện hai lần."
date: "2026-08-07T21:19:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "G"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 158
verified: false
draft: false
---

[CF 102536G - Phim điệp viên chung](https://codeforces.com/problemset/problem/102536/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi các diễn viên. Một dàn diễn viên là một tập hợp chính xác`g`diễn viên được chọn từ có sẵn`a`diễn viên. Lần diễn viên đầu tiên đã được cố định. Mỗi bộ phim tiếp theo phải có được bằng cách loại bỏ chính xác một diễn viên và thêm chính xác một diễn viên khác. Ngoài ra, không có dàn diễn viên nào có thể xuất hiện hai lần. 

Nhiệm vụ không phải là tìm một chuỗi cụ thể, chỉ bất kỳ chuỗi có độ dài hợp lệ nào`n`bắt đầu từ dàn diễn viên nhất định. 

Hạn chế quan trọng đó là`a`nhiều nhất là`1000`, trong khi`n`nhiều nhất là`10000`. Số lượng các lần chuyển đổi có thể có thể rất lớn, do đó việc tạo ra tất cả các lần chuyển đổi là không thể. Chúng tôi chỉ cần một tiền tố nhỏ của một thứ tự hợp lệ. Một giải pháp dành thời gian theo cấp số nhân để khám phá không gian trạng thái sẽ thất bại. 

Một lỗi phổ biến là liên tục thay thế cùng một diễn viên bằng diễn viên mới. Ví dụ, với`g = 2`và diễn viên`{a,b,c,d}`, trình tự`{a,b}`,`{a,c}`,`{a,d}`bị kẹt ngay lập tức mặc dù vẫn còn nhiều phôi chưa sử dụng. Vị trí thay đổi cuối cùng cũng phải di chuyển. 

Một sai lầm khác là quên rằng dàn diễn viên là một bộ. Việc xóa và thêm cùng một tác nhân sẽ không tạo ra thay đổi nào và việc xuất ra quá trình chuyển đổi như vậy sẽ vi phạm các quy tắc. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ coi mọi lần truyền có thể là một nút trong biểu đồ. Hai nút được kết nối nếu các diễn viên của chúng khác nhau bởi chính xác một tác nhân. Bắt đầu từ dàn diễn viên đã cho, chúng tôi có thể chạy DFS và tìm kiếm đường dẫn có độ dài`n`. Điều này đúng vì mọi cạnh đều thể hiện sự chuyển tiếp phim hợp pháp. 

Vấn đề là kích thước của biểu đồ này. Nó chứa`C(a,g)`trạng thái, quá lớn. Ngay cả việc kiểm tra tất cả các trạng thái lân cận nhiều lần cũng không thể thực hiện được khi`a`đạt tới`1000`. 

Quan sát hữu ích là các tập con có kích thước cố định. Các tập hợp con có kích thước cố định có thứ tự mã Gray trong đó các tập hợp con liên tiếp khác nhau bằng cách trao đổi chính xác một phần tử. Nếu chúng ta có thể liệt kê thứ tự như vậy và làm cho tập con đầu tiên của nó bằng với dàn diễn viên đã cho thì mỗi cặp liên tiếp sẽ tự động trở thành một chuyển tiếp phim hợp pháp. 

Chúng tôi làm điều này bằng cách sắp xếp lại các diễn viên. Nếu chúng ta tạo ra sự kết hợp kích thước`k`bắt đầu từ lần đầu tiên`k`các diễn viên, chúng ta có thể đặt các diễn viên được chọn ban đầu lên đầu tiên trong danh sách được sắp xếp lại. Khi`g`lớn, thay vào đó, việc tạo ra các diễn viên bị thiếu sẽ dễ dàng hơn vì việc thay thế một thành viên trong dàn diễn viên tương đương với việc thay thế một thành viên trong phần bổ sung của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(a,g) * a) | O(C(a,g)) | Quá chậm | 
| Tối ưu | O(n * g) | O(a + g) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu kích thước bó bột`g`nhỏ hơn hoặc bằng số lượng diễn viên bên ngoài dàn diễn viên, tạo ra sự kết hợp của các diễn viên bên trong dàn diễn viên. Nếu không thì tạo ra sự kết hợp của các diễn viên nằm ngoài dàn diễn viên. Biểu diễn phần bù giữ cho tập con được tạo ra nhỏ. 
2. Sắp xếp lại các tác nhân sao cho tập hợp con hiện tại xuất hiện dưới dạng tập hợp con đầu tiên`k`diễn viên. Mã màu xám kết thúc`k`diễn viên được chọn luôn bắt đầu với người đầu tiên`k`các vị trí. 
3. Tạo đệ quy mã Gray có kích thước cố định. Nửa đầu giữ diễn viên cuối cùng vắng mặt, và nửa sau giữ nó hiện diện trong khi đảo ngược hướng. Sự phản ánh này chính là nguyên nhân làm cho ranh giới giữa hai nửa chỉ thay đổi một tác nhân. 
4. Bỏ qua tập con được tạo đầu tiên vì đây là phim đầu tiên. Đối với mỗi tập hợp con sau, hãy so sánh nó với tập hợp con trước đó. Yếu tố biến mất là diễn viên rời đi, còn yếu tố mới là diễn viên tham gia. 
5. Dừng lại sau khi sản xuất`n - 1`chuyển tiếp. 

Tại sao nó hoạt động: thứ tự được tạo có bất biến là mọi tập hợp con liên tiếp đều khác nhau ở chính xác một phần tử được chọn. Vì phần tử được chọn đại diện cho các diễn viên trong dàn diễn viên hoặc các diễn viên bên ngoài dàn diễn viên, nên điều này luôn tương ứng với việc loại bỏ một diễn viên và thêm một diễn viên. Việc sắp xếp lại ban đầu làm cho tập hợp con được tạo đầu tiên bằng với phim đầu tiên được yêu cầu, do đó toàn bộ chuỗi bắt đầu chính xác. Việc tạo mã xám không bao giờ lặp lại một tập hợp con, vì vậy mỗi dàn diễn viên trong phim là duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gray_combinations(n, k, rev=False, start=0):
    if k == 0:
        yield ()
        return
    if k == n:
        yield tuple(range(start, start + n))
        return

    if not rev:
        yield from gray_combinations(n - 1, k, False, start)
        for x in gray_combinations(n - 1, k - 1, True, start):
            yield x + (start + n - 1,)
    else:
        for x in gray_combinations(n - 1, k - 1, False, start):
            yield x + (start + n - 1,)
        yield from gray_combinations(n - 1, k, True, start)

def solve():
    t = int(input())
    ans_all = []

    for case in range(t):
        g, n, a = map(int, input().split())
        actors = input().split()
        initial = input().split()

        initial_set = set(initial)

        if g <= a - g:
            small = initial[:]
            rest = [x for x in actors if x not in initial_set]
            order = small + rest
            k = g
            complement = False
        else:
            small = [x for x in actors if x not in initial_set]
            rest = initial[:]
            order = small + rest
            k = a - g
            complement = True

        previous = set(range(k))
        result = []
        count = 0

        for comb in gray_combinations(a, k):
            if count == 0:
                count += 1
                continue

            if count >= n:
                break

            current = set(comb)

            if complement:
                old = set(range(a)) - previous
                new = set(range(a)) - current
            else:
                old = previous
                new = current

            out_idx = next(iter(old - new))
            in_idx = next(iter(new - old))

            result.append(order[out_idx] + " " + order[in_idx])

            previous = current
            count += 1

        ans_all.append("\n".join(result))

    print("\n\n".join(ans_all))

solve()
```Trình tạo đệ quy là cốt lõi của giải pháp. Hướng không đảo ngược trước tiên tạo ra các tập hợp con không có tác nhân mới nhất, sau đó tạo các tập hợp con chứa tác nhân đó theo thứ tự ngược lại. Hướng ngược lại hoán đổi hai phần này. Sự đảo ngược đó là cần thiết vì tập con cuối cùng của nửa đầu và tập con đầu tiên của nửa sau chỉ khác nhau ở tác nhân mới nhất. 

Việc xử lý bổ sung là phần tinh tế. Khi chúng tôi tạo ra các tác nhân bên ngoài thay vì các tác nhân bên trong, tập hợp con được tạo sẽ mô tả các tác nhân bị thiếu trong dàn diễn viên. Sự thay đổi một yếu tố trong bộ còn thiếu vẫn chính xác là một diễn viên tham gia hoặc rời khỏi dàn diễn viên thực tế. 

Việc trích xuất chuyển tiếp sử dụng các khác biệt được thiết lập. Vì mã Gray đảm bảo chính xác một phần tử đã thay đổi nên cả hai điểm khác biệt đều chứa chính xác một tác nhân nên không cần tìm kiếm hoặc xác thực thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * a) | Mỗi bộ phim được sản xuất yêu cầu so sánh tối đa hai tập hợp con có kích thước`a`| 
| Không gian | O(a) | Chỉ trạng thái mã Gray hiện tại và danh sách tác nhân được lưu trữ | 

Sản lượng lớn nhất có thể chỉ là khoảng mười nghìn lần chuyển đổi, do đó hệ số tuyến tính về số lượng tác nhân dễ dàng nằm trong giới hạn. 

## Vỏ cạnh 

Khi nào`g = 1`, thuật toán vẫn hoạt động vì sự kết hợp một phần tử Mã Gray chỉ đơn giản duyệt qua từng tác nhân một. Quá trình chuyển đổi chỉ thay thế diễn viên đơn lẻ trước đó. 

Khi`g = a - 1`, thay vào đó, việc tạo diễn viên bị thiếu sẽ tránh được trường hợp khó tạo ra gần như toàn bộ tập hợp. Ví dụ, với năm diễn viên và bốn diễn viên, diễn viên bị thiếu sẽ thay đổi chính xác khi dàn diễn viên thay đổi. 

Khi dàn diễn viên ban đầu chứa các tác nhân không được sắp xếp theo thứ tự, giải pháp không phụ thuộc vào thứ tự đầu vào. đầu tiên`k`các diễn viên trong danh sách sắp xếp lại được chọn cụ thể sao cho tổ hợp được tạo đầu tiên là dàn diễn viên nhất định.
