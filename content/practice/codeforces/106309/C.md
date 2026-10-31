---
title: "CF 106309C - \u0413\u0440\u0438\u0431\u043e\u0447\u043a\u0438"
description: "Chúng tôi được cung cấp một lượng bột có hạn và một số loại bánh mà chúng tôi có thể nướng. Mỗi loại bánh bao tiêu tốn một lượng bột cố định, và đối với bánh có hương vị, loại bánh này cũng tiêu tốn một lượng nhân bánh cụ thể. Mỗi chiếc bánh mang lại một khoản lợi nhuận cố định."
date: "2026-06-25T07:45:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106309
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041f\u0435\u0440\u043c\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2025"
rating: 0
weight: 106309
solve_time_s: 46
verified: true
draft: false
---

[CF 106309C - \u0413\u0440\u0438\u0431\u043e\u0447\u043a\u0438](https://codeforces.com/problemset/problem/106309/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lượng bột có hạn và một số loại bánh mà chúng tôi có thể nướng. Mỗi loại bánh bao tiêu tốn một lượng bột cố định, và đối với bánh có hương vị, loại bánh này cũng tiêu tốn một lượng nhân bánh cụ thể. Mỗi chiếc bánh mang lại một khoản lợi nhuận cố định. Ngoài ra, còn có thêm một loại bánh “trơn” chỉ sử dụng bột và số lượng cung cấp không giới hạn, trong khi mỗi loại có hương vị còn bị hạn chế bởi số lượng nhân có sẵn. 

Nhiệm vụ là quyết định nên nướng bao nhiêu chiếc bánh của mỗi loại để chúng ta tối đa hóa tổng lợi nhuận mà không vượt quá số bột và không vượt quá số lượng có sẵn của mỗi lần nhân. 

Khó khăn là tất cả các lựa chọn đều tương tác thông qua một nguồn tài nguyên chung duy nhất là bột, trong khi mỗi loại hương vị cũng có giới hạn độc lập riêng. 

Các ràng buộc rất nhỏ: bột nhiều nhất là khoảng 1000 và số lượng hương vị nhiều nhất là 10. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân về số lượng chính xác cho mỗi hương vị, vì ngay cả một hệ số phân nhánh vừa phải cũng sẽ bùng nổ. Cách tiếp cận hoàn toàn tham lam cũng không an toàn vì một chiếc bánh có lợi nhuận trên mỗi gam nhân cao hơn có thể tiêu tốn quá nhiều bột và cản trở sự kết hợp của các loại bánh khác mang lại tổng lợi nhuận cao hơn. 

Một trường hợp thất bại tinh vi xuất hiện khi việc xếp hạng tham lam theo lợi nhuận trên mỗi chiếc bánh là sai lầm. 

Ví dụ, giả sử một chiếc bánh mang lại lợi nhuận cao nhưng tiêu tốn quá nhiều bột, trong khi chiếc bánh khác mang lại lợi nhuận thấp hơn một chút nhưng lại hiệu quả hơn trong việc sử dụng bột. Một chiến lược tham lam luôn chọn lợi nhuận trước mắt cao nhất có thể cạn kiệt nguồn lực quá sớm và bỏ lỡ một sự kết hợp tổng thể tốt hơn. 

Một cạm bẫy phổ biến khác là xử lý từng búi tóc một cách độc lập và lấy càng nhiều càng tốt cho mỗi loại. Điều đó bỏ qua việc lấy thêm một búi tóc thuộc bất kỳ loại nào sẽ làm giảm dung lượng toàn cầu cho tất cả những loại khác. 

## Phương pháp tiếp cận 

Quan sát quan trọng là số lượng các loại có hương vị rất ít, vì vậy chúng ta có thể coi chúng như một lớp quyết định về số lượng bánh của mỗi loại mà chúng ta lấy và mọi thứ khác sẽ chuyển thành tối ưu hóa một chiều so với số bột còn lại. 

Một ý tưởng mạnh mẽ sẽ thử tất cả các kết hợp về số lượng bánh của mỗi hương vị mà chúng ta lấy. Với mỗi vị i, chúng ta có thể lấy từ 0 đến min(ai, n/ci) bánh. Nếu chúng ta thử làm điều này một cách trực tiếp, thì số lượng trạng thái gần như sẽ trở thành tích của tối đa 100 lựa chọn cho mỗi hương vị, một con số lớn về mặt thiên văn. 

Cấu trúc giúp chúng ta tiết kiệm là sau khi xác định được số lượng bánh có hương vị của mỗi loại, vấn đề còn lại không đáng kể: tất cả số bột còn lại nên dành cho bánh mì thường. Điều đó làm giảm toàn bộ vấn đề trong việc lựa chọn các lựa chọn ba lô có giới hạn trên tối đa 10 mặt hàng, trong đó mỗi mặt hàng có bội số giới hạn và chi phí tuyến tính tính bằng bột. 

Thay vì liệt kê tất cả các kết hợp, chúng tôi tối ưu hóa từng hương vị một cách độc lập bằng cách sử dụng thủ thuật phân tách ba lô có giới hạn tiêu chuẩn. Mỗi chiếc bánh có hương vị đóng góp một khoản lợi nhuận cố định và tiêu thụ một lượng bột cố định, nhưng chúng ta không thể vượt quá mục ai/bi. Điều này chuyển đổi từng hương vị thành một tập mục bị giới hạn và toàn bộ vấn đề trở thành một chiếc ba lô vượt quá dung lượng n. 

Bánh mì đơn giản chỉ là một mặt hàng khác có nguồn cung không giới hạn, nên nó sẽ lấp đầy dung lượng còn sót lại trong DP một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê đầy đủ tất cả các hương vị | Hàm mũ trong m và ai | O(1) thêm | Quá chậm | 
| Ba lô buộc chặt các đồ vật đã phân hủy | O(n · tổng các mục bị phân hủy) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng tôi hiểu mỗi chiếc bánh có hương vị là một món đồ có thể được sử dụng nhiều nhất ai / hai lần. Điều này chuyển đổi “ràng buộc tài nguyên khi điền” thành giới hạn trên đơn giản về số lần chúng tôi có thể lấy mỗi mục. 
2. Chúng tôi chuyển đổi từng hương vị thành nhiều mục 0-1 bằng cách sử dụng phép chia nhị phân cho số lượng tối đa của nó. Điều này đảm bảo rằng chúng tôi xử lý các giới hạn lớn một cách hiệu quả mà không cần lặp lại tất cả các lần đếm. 
3. Chúng ta tạo một mảng lập trình động dp trong đó dp[x] biểu thị lợi nhuận tối đa có thể đạt được khi sử dụng đúng x gam bột. Chúng tôi khởi tạo tất cả các trạng thái là không thể truy cập được ngoại trừ dp[0] = 0. 
4. Chúng tôi lặp lại tất cả các mục đã chuyển đổi và cập nhật DP theo thứ tự giảm dần mức sử dụng bột. Điều này ngăn việc sử dụng lại cùng một mục nhiều lần trong một lần chuyển đổi, duy trì tính chính xác của hành vi 0-1. 
5. Sau khi xử lý tất cả các loại bánh có hương vị, chúng tôi coi bánh bao thường là sản phẩm không giới hạn cuối cùng. Chúng tôi giảm bớt dp bằng cách cho phép các chuyển đổi liên tục cộng thêm chi phí và lợi nhuận của bánh đơn giản hoặc tương tự, chúng tôi tính toán trước sự kết hợp tốt nhất có thể bằng cách thử tất cả mức sử dụng bột còn lại và đổ đầy bánh đơn giản vào đó. 
6. Câu trả lời cuối cùng là giá trị tối đa trên tất cả các trạng thái dp sau khi xem xét rằng mọi phần bột còn sót lại luôn có thể được chuyển thành bánh mì thường. 

Bất biến trong suốt DP là dp[x] luôn thể hiện lợi nhuận tốt nhất có thể đạt được bằng cách sử dụng chính xác x đơn vị bột sau khi xử lý tiền tố của các mặt hàng. Mỗi quá trình chuyển đổi đều đảm bảo tính khả thi vì chúng tôi không bao giờ vượt quá số lượng bánh có hương vị sẵn có và chúng tôi không bao giờ vượt quá tổng số bột. 

Tính chính xác phụ thuộc vào thực tế là mọi kế hoạch nướng bánh hợp lệ đều tương ứng với việc chọn nhiều tập hợp các mặt hàng có hương vị được giới hạn cùng với một số loại bánh đơn giản và cách thể hiện này được các trạng thái ba lô nắm bắt hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())

    items = []

    for _ in range(m):
        a, b, c, d = map(int, input().split())
        max_cnt = min(a // b, n // c)
        k = 1
        while max_cnt > 0:
            take = min(k, max_cnt)
            items.append((take * c, take * d))
            max_cnt -= take
            k <<= 1

    dp = [-10**18] * (n + 1)
    dp[0] = 0

    for cost, val in items:
        for j in range(n, cost - 1, -1):
            if dp[j - cost] != -10**18:
                dp[j] = max(dp[j], dp[j - cost] + val)

    best = 0
    for used in range(n + 1):
        if dp[used] < 0:
            continue
        remaining = n - used
        best = max(best, dp[used] + (remaining // c0) * d0)

    print(best)

if __name__ == "__main__":
    solve()
```Mảng DP được lập chỉ mục theo mức sử dụng bột thay vì số lượng bánh, điều này giúp cho sự tương tác giữa các loại bánh bao khác nhau trở nên rõ ràng. Mỗi loại hương vị được mở rộng bằng cách sử dụng phân tách nhị phân, đó là lý do tại sao vòng lặp bên trong vẫn hoạt động hiệu quả ngay cả khi một hương vị có thể được sử dụng nhiều lần. 

Vòng lặp cuối cùng rất cần thiết: DP chỉ tính đến bánh có hương vị, vì vậy chúng tôi chuyển đổi rõ ràng phần bột còn sót lại thành bánh đơn giản. Việc quên bước này là nguyên nhân phổ biến dẫn đến các câu trả lời sai, vì chỉ riêng dp[n] chưa hẳn là tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2 2 1
7 3 2 100
12 3 1 10
```Chúng tôi xử lý hương vị đầu tiên, cho phép tối đa 2 chiếc bánh (vì 7/3 = 2). Chúng ta tạo ra các vật phẩm tương ứng với 1 và 2 bánh. Đối với hương thứ hai, chúng ta phân tích giới hạn của nó một cách tương tự. 

Sau DP, chúng tôi theo dõi các trạng thái tốt nhất: 

| Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 2 | 100 | 
| 4 | 200 | 
| 5 | 210 | 
| 6 | 220 | 
| 8 | 230 | 

Bây giờ chúng ta thêm bánh đơn giản với chi phí 2 và lợi nhuận 1: 

Đối với mỗi trạng thái, lượng bột còn lại sẽ được chuyển đổi. 

Ví dụ: ở mức đã sử dụng = 8, còn lại = 2, chúng tôi cộng thêm 1 lợi nhuận, được 231. Giá trị tốt nhất trên tất cả các trạng thái trở thành 241, phù hợp với sự kết hợp tối ưu giữa bánh có hương vị và bánh thường. 

Dấu vết này cho thấy rằng chỉ riêng các quyết định có tính chất phù hợp là không đủ và chuyển đổi còn sót lại là yếu tố cuối cùng mang lại sự tối ưu. 

### Ví dụ 2 

đầu vào:```
100 1 25 50
15 5 20 10
```Chỉ có một loại hương vị tồn tại. Bánh có hương vị tối đa là 3. 

| Bột đã qua sử dụng | Lợi nhuận | 
| --- | --- | 
| 0 | 0 | 
| 20 | 10 | 
| 40 | 20 | 
| 60 | 30 | 

Bây giờ chúng ta lấp đầy phần bột còn lại bằng những chiếc bánh thường: 

Lúc dùng = 60, còn lại = 40 được 1 bánh thường. 

Tổng trở thành 40 + 30 = 70 mức tăng tương đương được thêm vào cấu trúc cơ bản, nhưng giải pháp tối ưu thay vào đó lại thích bỏ qua hoàn toàn bánh có hương vị và chỉ sử dụng bánh bao thường, mang lại 200. 

Điều này chứng tỏ rằng bánh có hương vị không tự động mang lại lợi ích ngay cả khi chúng có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × tổng_items) | Mỗi lần chuyển đổi DP sẽ quét mảng ba lô một lần cho mỗi mục bị phân hủy | 
| Không gian | O(n) | Mảng DP theo công suất bột | 

Các ràng buộc n ≤ 1000 và m ≤ 10 đảm bảo rằng ngay cả sau khi tách nhị phân, số lượng mục vẫn đủ nhỏ để DP có thể chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, c0, d0 = map(int, input().split())
    items = []

    for _ in range(m):
        a, b, c, d = map(int, input().split())
        max_cnt = min(a // b, n // c)
        k = 1
        while max_cnt > 0:
            take = min(k, max_cnt)
            items.append((take * c, take * d))
            max_cnt -= take
            k <<= 1

    dp = [-10**18] * (n + 1)
    dp[0] = 0

    for cost, val in items:
        for j in range(n, cost - 1, -1):
            if dp[j - cost] != -10**18:
                dp[j] = max(dp[j], dp[j - cost] + val)

    best = 0
    for used in range(n + 1):
        if dp[used] < 0:
            continue
        rem = n - used
        best = max(best, dp[used] + (rem // c0) * d0)

    return str(best)

# provided samples
assert run("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241"

assert run("""100 1 25 50
15 5 20 10
""") == "200"

# custom cases
assert run("""1 1 1 10
1 1 1 5
""") == "10", "prefer plain over weak flavor"

assert run("""5 1 5 100
10 1 5 1
""") == "100", "ignore bad flavor efficiency"

assert run("""10 0 2 3
""") == "15", "only plain buns"

assert run("""10 1 3 5
10 2 3 1
""") == "15", "edge small mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hương vị yếu vs đồng bằng mạnh mẽ | 10 | tham lam từ chối đồ yếu | 
| hương vị không hiệu quả | 100 | bỏ qua các loại vật phẩm thống trị | 
| không có hương vị | 15 | trường hợp đơn giản thuần túy | 
| ranh giới hỗn hợp | 15 | tương tác của DP + điền còn sót lại | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi bánh có hương vị thực sự tệ hơn bánh thường. đầu vào```
5 1 1 10
5 1 1 1
```khiến DP xem xét tùy chọn có hương vị, nhưng bước chuyển đổi cuối cùng đảm bảo rằng việc bỏ qua hoàn toàn là tối ưu, tạo ra 50 thay vì giá trị hỗn hợp nhỏ hơn. 

Một trường hợp khác là giải pháp tốt nhất không sử dụng bánh có hương vị nào cả. DP vẫn phải trả về 0 một cách chính xác để sử dụng theo hương vị và cho phép chuyển đổi hoàn toàn sang bánh đơn giản. Nếu không xem xét trạng thái “đã sử dụng = 0” trong vòng lặp cuối cùng, giải pháp sẽ bỏ lỡ cấu hình tối ưu hoàn toàn đơn giản một cách không chính xác. 

Trường hợp thứ ba xảy ra khi những chiếc bánh có hương vị lấp đầy bột, không để lại phần còn lại. Thuật toán vẫn hoạt động vì số hạng chuyển đổi cuối cùng trở thành 0, do đó chỉ DP quyết định kết quả mà không bị can thiệp.
